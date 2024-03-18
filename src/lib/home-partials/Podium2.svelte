<script lang="ts">
    import { Chart, type EChartsOptions } from 'svelte-echarts';
    import podium from '$lib/data/type_resto_podium.json';
    import podium_cuisine from '$lib/data/cuisine_podium.json';
    import { choixLieuStore } from '$lib/stores/stores';

    let choix_lieu = $choixLieuStore;
    let options: EChartsOptions;

    $: {
        choix_lieu = $choixLieuStore;

        const categories_cuisine = podium_cuisine.filter(d => d.lieu === choix_lieu).map(d => d.type)

 

// Définir les données de la série

const data_cuisine = podium_cuisine.filter(d => d.lieu === choix_lieu).map(d => d.pct)

 

// Définir les options du graphique

options = {

    color: ['#8B0000'],

    //, ['#C0C0C0','#FFD700', '#CD7F32'],

    xAxis: {

        type: 'category',

        data: podium_cuisine.filter(d => d.lieu === choix_lieu).map(d => d.type),

        show: false

    },

    yAxis: {

        type: 'value',

        axisLabel: {

            formatter: '{value} %'

        }

    },

    series: [{

        name: 'pct',

        type: 'bar',

        data: data_cuisine,

        label: {

            show: true,

            position: 'top',

            formatter: '{b}'

        }

    }]

};
    }
</script>

<div class="podium">
    <Chart {options} />
</div>

<style>
    .podium {
        width: 50vw;
        height: 70vh;
    }
</style>
