<script lang="ts">
    import * as d3 from 'd3';
  import { onMount } from 'svelte';
  import { draw, fade } from 'svelte/transition';

    let data: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    export let width = 640;
    export let height = 400;
    export let marginTop = 20;
    export let marginRight = 20;
    export let marginBottom = 20;
    export let marginLeft = 20;

    $: x = d3.scaleLinear([0, data.length - 1], [marginLeft, width - marginRight]);
    $: y = d3.scaleLinear(d3.extent(data) as [number, number], [height - marginBottom, marginTop]);
    $: line = d3.line((d, i) => x(i), y);

    let gx: any;
    let gy: any;
    $: d3.select(gy).call(d3.axisLeft(y));
      $: d3.select(gx).call(d3.axisBottom(x));

    let show = false;
    onMount(() => show = true);
</script>

<!-- Add chart -->
<svg width={width} height={height}>
    <!-- Add x-axis -->
    <g bind:this={gx} transform="translate(0,{height - marginBottom})" color="white" />
    <!-- Add y-axis -->
    <g bind:this={gy} transform="translate({marginLeft},0)" color="white" />
    <!-- Add line -->
    {#if show}
        <path stroke-width="1.5" d={line(data)} stroke="white" 
        transition:draw={{easing: t => t, duration: data.length * 200}}/>
    {/if}
    <!-- Add data points -->
    <g stroke-width="1.5">
        {#each data as d, i}
        {#if show}
            <circle cx={x(i)} cy={y(d)} r="2.5" fill="white" 
            transition:fade={{delay: i * 200}}/>
        {/if}
        {/each}
    </g>
</svg>