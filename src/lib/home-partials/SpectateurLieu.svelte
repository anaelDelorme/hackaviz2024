<script lang="ts">
import { ProgressRadial, RangeSlider  } from '@skeletonlabs/skeleton';
let value_heures_paris: number = 43; 
let value_heures_spectateurs_paris: number = 57; 
let value_heures_medailles_paris: number = 58; 
import { Chart, type EChartsOptions } from 'svelte-echarts'
import jo_horraire_ville from '$lib/data/jo_horraire_ville.json';
import dayjs from 'dayjs';
import 'dayjs/locale/fr';
import Carto from '$lib/home-partials/Carto.svelte';
import { horaires_store } from '$lib/stores/stores';
const timeCurrent = $horaires_store;
console.log("timeCurrent",timeCurrent)
dayjs.locale('fr');

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
let targetHoraire = '2024-07-24 10:00:00+00:00';
interface AggregatedData {
  [key: string]: number;
}

let aggregatedData: AggregatedData = {}; // Initialisez avec une valeur vide
  let filteredData: AggregatedJoData[] = []; 

  $: {
    const targetHoraire = horaires[value_slider - 1]; // -1 car les indices commencent à 0
    aggregatedData = jo_horraire_ville
      .map(d => ({ ...d, capacité_h: Number(d.capacité_h) }))
      .filter(d => d.horraire === targetHoraire)
      .reduce((acc, curr) => {
        acc[curr.ville] = (acc[curr.ville] || 0) + curr.capacité_h;
        return acc;
      }, {} as AggregatedData);

    filteredData = Object.entries(aggregatedData).map(([ville, capacité_h]) => ({
      ville,
      capacité_h,
    }));
    
    updateChartOptions(); // Mettez à jour les options du graphique
  }


function formatNumberWithSpaces(num) {
  const value = Math.round(num);
  return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
}
  
//slider
let horaires = Array.from(new Set(jo_horraire_ville.map(d => d.horraire)));
console.log("horaires",horaires);
horaires.sort((a, b) => a - b);
console.log("horaires",horaires);

let min = horaires[0];

let max = horaires.length - 1;

let initialValue = 0;
let value_slider = horaires.findIndex(h => h >= initialValue) + 1;
console.log("value_slider",value_slider); 

  let options: EChartsOptions = {
	xAxis: {
	  type: 'value',
    axisLabel: {
      formatter: function (value) {
        return formatNumberWithSpaces(value);
      }
    }
	},
	yAxis: {
	  type: 'category',
	  data: filteredData.map(d => d.ville),
    axisLabel: {
      formatter: function (value) {
        const index = value.indexOf('(');
        return index !== -1 ? value.substring(0, index).trim() : value;
      }
    }
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
	  bottom: '10%',
    
	  containLabel: true
	},
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'shadow'
        },
        formatter: function (params) {
            const value = params[0].value; // obtenir la valeur
            const formattedValue = formatNumberWithSpaces(value); // formater la valeur
            return `<p>${params[0].name}</p>
                    <p>${formattedValue}</p>`; // retourner le tooltip avec le nom et la valeur formatée
          }
    }
  }

  function updateChartOptions() {
    $horaires_store = horaires[value_slider];
    console.log("New store",$horaires_store);
    const updatedOptions: EChartsOptions = {
      ...options, // Copier les options existantes
      yAxis: {
        type: 'category',
        data: filteredData.map(d => d.ville),
        axisLabel: {
          formatter: function (value) {
            const index = value.indexOf('(');
            return index !== -1 ? value.substring(0, index).trim() : value;
          }
        }
      },
      series: [{
        data: filteredData.map(d => d.capacité_h),
        type: 'bar',
        itemStyle: {
          color: "#06b6d4"
        }
      }]
    };
    options = updatedOptions; // Mettre à jour les options du graphique
  }

    let isPlaying = false; // État de lecture

   function startPlayback() {
    isPlaying = !isPlaying;;
    let currentIndex = value_slider;
    const interval = setInterval(() => {
      if (isPlaying) {
        currentIndex = (currentIndex + 1) % (max + 1); // Boucler de la première à la dernière valeur du slider
        value_slider = currentIndex;
      } else {
        clearInterval(interval); // Arrêter l'intervalle lorsque la lecture est mise en pause
      }
    }, 1000); // Délai d'une seconde
  } 


  
</script>

<div class=' mx-4 lg:mx-12 '>


<div class="grid grid-cols-1 sm:grid-cols-4 gap-4 text-center">
    <div>
      <h3 class="h3 text-center">
    <span class="bg-gradient-to-br from-secondary-900 to-secondary-600 bg-clip-text text-transparent box-decoration-clone">Les Jeux de Paris... vraiment ?</span>
</h3>

<p class="text-left"> Saviez-vous que seulement <span class="em-secondary" role="presentation">58 %</span> des sessions des jeux olympiques et paralympiques auront lieu à Paris ?</p>
  <p class="text-left"> Et si vous deviez relier chaque site de compétition, il vous faudrait parcourir <span class="em-secondary" role="presentation">37 070 km</span>. En restant dans l'hexagone, le voyage serait de <span class="em-secondary" role="presentation">2 560 km</span>, ou l'équivalent de <span class="em-secondary" role="presentation">610 marathons</span>.
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
<h2 class="mt-8"></h2>
  <div class="w-0.8 mb-4">
 
    <RangeSlider name="range-slider" bind:value={value_slider} min={min} max={max} step={1} ticked>
      <div class="flex justify-between items-center">
        <div class="font-bold">Regardons plus en détail, selon chaque jour de compétition :</div>
        <button class="btn bg-surface-300" on:click={startPlayback}>
    {isPlaying ? 'Pause' : 'Démarrer'}
        </button>
        <div class="text-xs">{dayjs(horaires[value_slider]).subtract(2, "h").format('dddd D MMMM [à] hh:mm')}</div>      </div>
    </RangeSlider>
    
  </div>

<div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-center">
    <div class="card shadow-xl bg-secondary-50">
      <Carto />
    </div>
<div class="card shadow-xl bg-secondary-50 h-screen sm:h-auto">
    <br/>
        <h4>Nombre de spectateurs par ville et par période</h4>
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