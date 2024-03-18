<script lang="ts">
  import { Chart, type EChartsOptions } from 'svelte-echarts';
  import podium from '$lib/data/type_resto_podium.json';
  import coordonnees_resto from '$lib/data/coordonnees_resto.json';
  import coordonnees_lieux from '$lib/data/coordonnees_lieux.json';
  import { onMount, onDestroy } from 'svelte';
  import { Map, MapStyle, Marker, config } from '@maptiler/sdk';
  import "@maptiler/sdk/dist/maptiler-sdk.css";
  import { choixLieuStore } from '$lib/stores/stores';

  let map: Map | undefined;
  let mapContainer: HTMLDivElement | null = null;
  let choix_lieu = $choixLieuStore;

  // Configure API key
  config.apiKey = 'KdoI86QumVIcjl6fU3eI';

  // Watch for changes in choix_lieuStore
  $: {
    choix_lieu = $choixLieuStore;
        console.log("sdqd",choix_lieu);

    const lat = coordonnees_lieux.filter(d => d.lieu === choix_lieu).map(d => d.latitude)[0];
    const long = coordonnees_lieux.filter(d => d.lieu === choix_lieu).map(d => d.longitude)[0];
    console.log("TTTTTTTTTTTT",lat, long);
      const initialState = { lng: long, lat: lat, zoom: 14 };

      if (mapContainer) {
        if (!map) {
          map = new Map({
            container: mapContainer,
            style: MapStyle.STREETS,
            center: [initialState.lng, initialState.lat],
            zoom: initialState.zoom
          });
        }

        new Marker({ color: "#FF0000" })
          .setLngLat([long, lat])
          .addTo(map);
      
    }
  }
</script>

<div class="map-wrap">
  <div class="map" bind:this={mapContainer}></div>
</div>

<style>
  .map-wrap {
    width: 50vw;
    height: 70vh;
  }
</style>
