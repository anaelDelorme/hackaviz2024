<script lang="ts">
  import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import { popup } from '@skeletonlabs/skeleton';
  import type { PopupSettings } from '@skeletonlabs/skeleton';
  import jo_horraire_ville from '$lib/data/jo_horraire_ville.json';
  import { horaires_store } from '$lib/stores/stores';
  let timeCurrent = $horaires_store;
  console.log("timeCurrent carto",timeCurrent)
  let localisation_lieux: any[];
  let Tooltip: any; // Déclarez la variable Tooltip ici

  interface Marker {
    long: number;
    lat: number;
    name: string;
    capacity: number;
  }

  function getUniqueCities(data: any[]): string[] {
    return [...new Set(data.map((d) => d.ville))];
  }

 function getCityCoordinates(city: string, loc: any): [number, number] | null {
  if (loc && Array.isArray(loc.features)) {
    const location = loc.features.find((d) => d.properties && d.properties.ville === city);

    if (location) {
      return [location.geometry.coordinates[0], location.geometry.coordinates[1]];
      
    }
    
  }
  return null;
}

  function getMarkersFromData(data: any[], time: string, localisationData: any[]): Marker[] {
    if (!localisationData) {
      return [];
    }
    const filteredData = data.filter((d) => d.horraire === time);

    const groupedData = filteredData.reduce<{ [key: string]: number }>((acc, curr) => {
      const key = curr.ville;
      acc[key] = (acc[key] || 0) + curr.capacité_h;
      return acc;
    }, {});

    const uniqueCities = getUniqueCities(data);
    const markers: Marker[] = [];

    uniqueCities.forEach((city) => {
      const coordinates = getCityCoordinates(city, localisationData);
      if (coordinates) {
        markers.push({
          long: coordinates[0],
          lat: coordinates[1],
          name: city,
          capacity: groupedData[city] || 0,
        });
      }
    });

    return markers;
  }

  function showPopup(d: any) {
    const settings: PopupSettings = {
      title: d.name,
      content: `Nombre de spectateurs: ${d.capacity}`,
      actions: [
        {
          text: 'Close',
          theme: 'secondary',
          handler: () => {}
        }
      ]
    };

    popup.show(settings);
  }

  let svg: any;
  let projection: any;


        function mouseover(event, d) {
          Tooltip.style("opacity", 1);
        }

function mousemove(event, d) {
  const formattedCapacity = d.capacity.toLocaleString(); // Format avec séparateur de milliers
  const xOffset = -500; // Décalage horizontal par rapport à la souris
  const yOffset = -400; // Décalage vertical par rapport à la souris

  Tooltip
    .html(`<div class="text-center font-bold">${d.name}</div>Nombre de spectateurs: ${formattedCapacity}`)
    .style("transform", `translate(${event.pageX + xOffset}px, ${event.pageY + yOffset}px)`)
    .style("opacity", 1); // Assurez-vous que la popup est visible
}



        function mouseleave(event, d) {
          Tooltip.style("opacity", 0);
        }


  onMount(() => {
    if (!d3.select('#carto_spectateur').empty()) {
      svg = d3.select('#carto_spectateur');
      const width = +svg.attr('width');
      const height = +svg.attr('height');

      Promise.all([
        d3.json("localisation_lieux.geojson"),
        d3.json("habillage_administratif_fr.geojson"),
        d3.json("libelle_administratif_fr.geojson")
      ]).then(([localisationData, mapData, libelle]) => {
        localisation_lieux = localisationData;
        const time = timeCurrent;
        const markers = getMarkersFromData(jo_horraire_ville, time, localisationData);
        const maxCapacity = Math.max(...markers.map((m) => m.capacity));

        projection = d3.geoIdentity()
          .reflectY(true)
          .fitSize([width, height], mapData);

        svg.selectAll("*").remove(); // Clear previous content

        // Draw the map
        svg.append("g")
          .selectAll("path")
          .data(mapData.features)
          .enter()
          .append("path")
          .attr("fill", "#69b3a2")
          .attr("d", d3.geoPath().projection(projection))
          .style("stroke", "none");

        svg.selectAll("text")
            .data(libelle.features)
            .enter()
            .append("text")
            .text(d => d.properties.emplacement)
            .attr("x", d => projection(d.geometry.coordinates)[0])
            .attr("y", d => projection(d.geometry.coordinates)[1])
            .attr("text-anchor", "middle")
            .attr("font-size", "15px")
            .attr("fill", "darkslateblue");

        svg.selectAll(".circle")
          .data(markers)
          .enter()
          .append("circle")
          .attr("cx", d => projection([d.long, d.lat])[0])
          .attr("cy", d => projection([d.long, d.lat])[1])
          .attr("r", d => (d.capacity / maxCapacity) * 20)
          .attr("class", "circle")
          .style("fill", "#FFF")
          .attr("stroke", "#FFF")
          .attr("stroke-width", 3)
          .attr("fill-opacity", 0.4)
          .on("mouseover", mouseover)
          .on("mousemove", mousemove)
          .on("mouseleave", mouseleave);

        Tooltip = d3.select("#my_dataviz2")
          .append("div")
          .attr("class", "tooltip")
          .style("opacity", 0)
          .style("background-color", "white")
          .style("width", "20%")
          .style("border", "solid")
          .style("border-width", "2px")
          .style("border-radius", "5px")
          .style("padding", "5px");

      });



    } else {
      console.error("L'élément SVG n'existe pas dans le DOM");
    }
  });

        function clearSVG() {
  if (svg) {
    svg.selectAll("circle").remove();
  }
}
let monTime = $horaires_store;
function createCircles() {
  if (!projection) {
    return;
  }
  const markers = getMarkersFromData(jo_horraire_ville, monTime, localisation_lieux);
  const maxCapacity = Math.max(...markers.map((m) => m.capacity));

  svg.selectAll("circle").remove(); // Nettoyer les cercles existants

  svg.selectAll(".circle")
    .data(markers)
    .enter()
    .append("circle")
    .attr("cx", d => projection([d.long, d.lat])[0])
    .attr("cy", d => projection([d.long, d.lat])[1])
    .attr("r", d => (d.capacity / maxCapacity) * 20)
    .attr("class", "circle")
    .style("fill", "#FFF")
    .attr("stroke", "#FFF")
    .attr("stroke-width", 3)
    .attr("fill-opacity", 0.4)
    .on("mouseover", mouseover)
    .on("mousemove", mousemove)
    .on("mouseleave", mouseleave);
}
// Utilisez la logique réactive pour appeler la fonction de nettoyage du SVG
$: {
   monTime = $horaires_store;
  clearSVG();
  createCircles();
}

function updateMapOptions() {
    console.log("New new store",$horaires_store);


      }
</script>

<div id="my_dataviz2">
<svg id="carto_spectateur" width="600" height="400"></svg>
</div>

<style>
  #my_dataviz2 {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80vh; /* Utilisation de toute la hauteur de la fenêtre */
  }

  #carto_spectateur {
    /* Styles supplémentaires pour le SVG si nécessaire */
  }
</style>