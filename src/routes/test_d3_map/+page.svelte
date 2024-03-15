<script lang="ts">
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
  
    let svg: any;
  
    onMount(() => {
      if (!d3.select('#carto_spectateur').empty()) {
        svg = d3.select('#carto_spectateur');
        const width = +svg.attr('width');
        const height = +svg.attr('height');
        const projection = d3.geoMercator()
                .center([2, 47])                // GPS of location to zoom on
                .scale(100)                       // This is like the zoom
                
    d3.json("habillage_adminsitratif_fr_4326.geojson").then(data => {

    // Draw the map
    svg.append("g")
        .selectAll("path")
        .data(data.features)
        .enter()
        .append("path")
          .attr("fill", "grey")
          .attr("d", d3.geoPath()
              .projection(projection)
          )
        .style("stroke", "none");
        console.log("SVG",svg);
        
  });
      } else {
        console.error("L'élément SVG n'existe pas dans le DOM");
      }
    });

    
  </script>

<h1>Titre</h1>
<svg id="carto_spectateur" width="440" height="300"></svg>