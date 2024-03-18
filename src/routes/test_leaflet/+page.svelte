<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { Map, MapStyle, Marker, config } from '@maptiler/sdk';
  import "@maptiler/sdk/dist/maptiler-sdk.css";

  let map: Map | undefined;
  let mapContainer: HTMLDivElement | null = null;

  config.apiKey = 'KdoI86QumVIcjl6fU3eI';

  onMount(() => {
    const initialState = { lng: 139.753, lat: 35.6844, zoom: 14 };

    if (mapContainer) {
      map = new Map({
        container: mapContainer,
        style: MapStyle.STREETS,
        center: [initialState.lng, initialState.lat],
        zoom: initialState.zoom
      });

      new Marker({ color: "#FF0000" })
        .setLngLat([139.7525, 35.6846])
        .addTo(map);
    }
  });

  onDestroy(() => {
    if (map) {
      map.remove();
    }
  });
</script>

<div class="map-wrap">
  <div class="map" bind:this={mapContainer}></div>
</div>

<style>
  .map-wrap {
    position: relative;
    width: 100%;
    height: calc(100vh - 77px); /* calculate height of the screen minus the heading */
  }

  .map {
    position: absolute;
    width: 100%;
    height: 100%;
  }
</style>
