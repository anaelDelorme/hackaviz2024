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
		  color:  "#06b6d4"
		}
	  },
	],
	grid: {
	  left: '3%',
	  right: '4%',
	  bottom: '3%',
	  containLabel: true
	},
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow'
        }
    }
  }
</script>

<div class=' mx-4 lg:mx-12 '>


<div class="grid grid-cols-1 sm:grid-cols-4 gap-4 text-center">
    <div>
      <h3 class="h3 text-center">
    <span class="bg-gradient-to-br from-secondary-900 to-secondary-600 bg-clip-text text-transparent box-decoration-clone">Les Jeux de Paris... vraiment ?</span>
</h3>

<p class="text-left"> Saviez-vous que seulement <span class="em-secondary" role="presentation">58 %</span> des sessions des jeux olympiques et paralympiques auront lieu à Paris ?</p>
  <p class="text-left"> Et si vous deviez relier chaque site de compétition, il vous faudrait parcourir <span class="em-secondary" role="presentation">37 070 km</span>. En restant dans l'hexagone, le voyage serai de <span class="em-secondary" role="presentation">2 560 km</span>, ou l'équivalent de <span class="em-secondary" role="presentation">610 marathons</span>.
    </div>
    <div class="card shadow-xl bg-secondary-50">
        <section class="p-4 flex items-center justify-center">
            <ProgressRadial value={value_heures_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
                1 473
            </ProgressRadial>
        </section>
                <footer class="card-header">Heure d'épreuves en dehors de Paris</footer>

    </div>
    <div class="card shadow-xl bg-secondary-50">
        <section class="p-4 flex items-center justify-center">
            <ProgressRadial value={value_heures_spectateurs_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
                10,4 millions
            </ProgressRadial>
        </section>
                <footer class="card-header">Spectateurs en dehors de Paris</footer>

    </div>
    <div class="card shadow-xl bg-secondary-50">
        <section class="p-4 flex items-center justify-center">
        <ProgressRadial value={value_heures_medailles_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
            889
        </ProgressRadial>
    </section>
            <footer class="card-header">Médailles décernées en dehors de Paris</footer>

    </div>
</div>
<h2 class="mt-8">Pour aller plus loin : </h2>
<div class="grid grid-cols-2 gap-4 text-center">
    <div class="card shadow-xl bg-secondary-50">
        <img src="carto.svg" alt="Carto" style="max-height: 450px; width: auto;" />
    </div>
    <div class="card shadow-xl bg-secondary-50">
    <br/>
        <p>Nombre de spectateurs par ville et par période</p>
        <Chart {options} />
    </div>
</div>
</div>

<style lang="postcss">
    .em-primary {
        padding: 1px 4px 1px 4px;
        font-weight: bold;
        white-space: nowrap;
        background-color: rgba(var(--color-primary-900) / 1);
            color: white;
    }
    .em-secondary {
        padding: 1px 4px 1px 4px;
        font-weight: bold;
        white-space: nowrap;
        background-color: rgba(var(--color-secondary-900) / 1);
            color: white;
    }
</style>