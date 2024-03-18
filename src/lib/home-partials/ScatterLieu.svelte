<script lang="ts">
    import { Chart, type EChartsOptions } from 'svelte-echarts';
// prettier-ignore
import type_resto_scatter from '$lib/data/type_resto_scatter.json';
import cuisine_scatter from '$lib/data/cuisine_scatter.json';
  import { choixLieuStore } from '$lib/stores/stores';

let choix_lieu = $choixLieuStore;
let options: EChartsOptions ;



$:{
    choix_lieu = $choixLieuStore;
    options= { 

  grid: {
        left: '10%',
        right: '20%',
        bottom: '15%',
        containLabel: true
    },

    xAxis: {
    name: 'Part de \ncuisine \nfrançaise',
    min: 0, max: 12,
    axisLine: {
            symbol: ['none', 'arrow'],
            symbolSize: [8, 16],
            lineStyle: {
                color: '#333'
                }
            }
    },
  yAxis: {
    name: 'Part de \ncuisine \ndu monde',
    min: 0, max: 60,
    axisLine: {
            symbol: ['none', 'arrow'],
            symbolSize: [8, 16],
            lineStyle: {
                color: '#333'
                }
            }
    },
  series: [
    {
      symbolSize: 8,
    color: '#a03a6c',
      data: cuisine_scatter.filter(d => d.lieu !== choix_lieu).map( d => [d.cuisine_francaise, d.cuisine_du_monde]),
      type: 'scatter'
    },{
    symbolSize: 20,
    color: '#2E8B57',
    data: cuisine_scatter.filter(d => d.lieu === choix_lieu).map( d => [d.cuisine_francaise, d.cuisine_du_monde]),
    type : 'effectScatter' }
  ],
};
}


  </script>
  
<div class="scatter">
    <Chart {options} />
</div>
  
  <style>
    .scatter {
      width: 200vh;
      height: 70vh;
    }
  </style>
  