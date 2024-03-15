<script lang="ts">
    import { Chart, type EChartsOptions } from 'svelte-echarts'
    import jo_horraire_ville from '$lib/data/jo_horraire_ville.json';
  
    const jo_horraire_ville_data = jo_horraire_ville.map(d => [d.ville, d.capacité_h]);
  
    const options: EChartsOptions = {
      xAxis: {
        type: 'value',
      },
      yAxis: {
        type: 'category',
        data: jo_horraire_ville_data.map(d => d[0]),
      },
      series: [
        {
          data: jo_horraire_ville_data.map(d => d[1]),
          type: 'bar',
          itemStyle: {
            color: '#4190f6'
          }
        },
      ],
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
      }
    }
  </script>
  
  <div class="app">
    <Chart {options} />
  </div>
  
  <style>
    .app {
      width: 100vw;
      height: 100vh;
    }
  </style>
  