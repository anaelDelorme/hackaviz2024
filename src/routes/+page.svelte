<script lang="ts">

import { ProgressRadial } from '@skeletonlabs/skeleton';
let value_heures_paris: number = 43; 
let value_heures_spectateurs_paris: number = 57; 
let value_heures_medailles_paris: number = 58; 
import { Chart, type EChartsOptions } from 'svelte-echarts'
import jo_horraire_ville from '$lib/data/jo_horraire_ville.json';

// Graph spectateur par lieu et par data
interface JoData {
  id: number;
  Jeux: string;
  Discipline: string;
  Épreuve: string;
  Phase: string;
  Genre: string;
  Début: string;
  Fin: string;
  Lieu: string;
  horraire: string;
  capacité_h: number;
  ville: string;
  Enjeu: string;
};
const targetHoraire = '2024-07-28 10:00:00+00:00';
interface AggregatedData {
  [key: string]: number;
}
const aggregatedData: AggregatedData = jo_horraire_ville
  .map(d => ({ ...d, capacité_h: Number(d.capacité_h) }))
  .filter(d => d.horraire === targetHoraire)
  .reduce((acc, curr) => {
    if (acc[curr.ville]) {
      acc[curr.ville] += curr.capacité_h;
    } else {
      acc[curr.ville] = curr.capacité_h;
    }
    return acc;
  }, {} as AggregatedData);

interface AggregatedJoData {
  ville: string;
  capacité_h: number;
}
const filteredData: AggregatedJoData[] = Object.entries(aggregatedData).map(([ville, capacité_h]) => ({
  ville,
  capacité_h,
}));
  
console.log("filteredData", filteredData);
  const options: EChartsOptions = {
	xAxis: {
	  type: 'value',
	},
	yAxis: {
	  type: 'category',
	  data: filteredData.map(d => d.ville),
	},
	series: [
	  {
		data: filteredData.map(d => d.capacité_h),
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
	  containLabel: true,
	  height:'90%'
	}
  }

</script>

<div class="container mx-auto">
	<section class="w-full">
		<h1 class="h1">
			<span class="bg-gradient-to-br from-blue-900 to-cyan-600 bg-clip-text text-transparent box-decoration-clone">Les Jeux de Paris, vraiment ?</span>
		</h1>
		<br/>
		<p> Saviez-vous que seulement <span class="em" style="background-color:dodgerblue ; color: white;" role="presentation">58% des sessions</span>des jeux olympiques et paralympiques auront lieu à Paris ?</p>
		<br/>
		<h2>Chiffres clés :</h2>
		<div class="grid grid-cols-4 gap-4  text-center">
			<div class="card">
				<header class="card-header">Nombre d’heures d’épreuves en dehors de Paris</header>
				<section class="p-4 flex items-center justify-center">
					<ProgressRadial value={value_heures_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
						1 473 heures
					</ProgressRadial>
				</section>
			</div>
			<div class="card">
				<header class="card-header">Nombre de spectateurs en dehors de Paris</header>
				<section class="p-4 flex items-center justify-center">
					<ProgressRadial value={value_heures_spectateurs_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
						10,4 millions
					</ProgressRadial>
				</section>
			</div>
			<div class="card">
				<header class="card-header">Nombre de médailles décernées en dehors de Paris</header>
				<section class="p-4 flex items-center justify-center">
				<ProgressRadial value={value_heures_medailles_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
					889 médailles
				</ProgressRadial>
			</section>
			</div>
		</div>
		<h2>Pour aller plus loin : </h2>
		<div class="grid grid-cols-2 gap-4 text-center">
			<div class="card">
				<img src="carto.svg" alt="Carto" style="max-height: 450px; width: auto;" />
			</div>
			<div class="card">
			<br/>
				<p>Nombre de spectateurs par ville et par période</p>
				<Chart {options} />
			</div>
	</section>
	<section class="w-full">
		<h1 class="h1">
		<span class="bg-gradient-to-br from-red-500 to-yellow-500 bg-clip-text text-transparent box-decoration-clone">La France, capitale de la Gastronomie ?</span>
	</h1>
</section>
<section class="w-full">
	<h1 class="h1">
		<span class="bg-gradient-to-br from-pink-500 to-violet-500 bg-clip-text text-transparent box-decoration-clone">Préparez votre programme télé !</span>
	</h1>
	<p>
		Toutes les disciplines n’auront pas la même visibilité. Si l’athlétisme est la championne des sessions en prime-time, certaines disciplines sont plutôt programmées en journée.
	</p>
</section>
</div>

<style lang="postcss">
      .em {
          padding: 1px 4px 1px 4px;
          font-weight: bold;
		  white-space: nowrap;
      }

	figure {
		@apply flex relative flex-col;
	}
	figure svg,
	.img-bg {
		@apply w-64 h-64 md:w-80 md:h-80;
	}
	.img-bg {
		@apply absolute z-[-1] rounded-full blur-[50px] transition-all;
		animation: pulse 5s cubic-bezier(0, 0, 0, 0.5) infinite,
			glow 5s linear infinite;
	}
	@keyframes glow {
		0% {
			@apply bg-primary-400/50;
		}
		33% {
			@apply bg-secondary-400/50;
		}
		66% {
			@apply bg-tertiary-400/50;
		}
		100% {
			@apply bg-primary-400/50;
		}
	}
	@keyframes pulse {
		50% {
			transform: scale(1.5);
		}
	}
</style>
