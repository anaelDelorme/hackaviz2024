<script lang="ts">
    import { Chart, type EChartsOptions } from 'svelte-echarts';
// prettier-ignore
import type_resto_scatter from '$lib/data/type_resto_scatter.json';
import cuisine_scatter from '$lib/data/cuisine_scatter.json';
const choix_lieu = "Arena Paris Nord";
const options: EChartsOptions = { 

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

    color: '#8B0000',

      data: cuisine_scatter.filter(d => d.lieu !== choix_lieu).map( d => [d.cuisine_francaise, d.cuisine_du_monde]),

      type: 'scatter'

    },{

    symbolSize: 20,

    color: '#2E8B57',

    data: cuisine_scatter.filter(d => d.lieu === choix_lieu).map( d => [d.cuisine_francaise, d.cuisine_du_monde]),

    type : 'effectScatter' }

  ],

};




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
  