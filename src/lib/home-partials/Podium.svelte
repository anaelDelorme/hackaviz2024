<script lang="ts">
    import { Chart, type EChartsOptions } from 'svelte-echarts';
    import podium from '$lib/data/type_resto_podium.json';
    import podium_cuisine from '$lib/data/cuisine_podium.json';
    import { choixLieuStore } from '$lib/stores/stores';

    let choix_lieu = $choixLieuStore;
    let options: EChartsOptions;

    $: {
        choix_lieu = $choixLieuStore;
        const filteredPodium = podium.filter(d => d.lieu === choix_lieu);
        const categories = filteredPodium.map(d => d.type);
        const data = filteredPodium.map(d => d.pct);
        options = {
            color: ['#8B0000'],
            xAxis: {
                type: 'category',
                data: categories,
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
                data: data,
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
