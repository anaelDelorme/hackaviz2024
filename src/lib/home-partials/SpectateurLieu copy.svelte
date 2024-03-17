<script lang="ts">
import { ProgressRadial, ListBox, ListBoxItem } from '@skeletonlabs/skeleton';
let value_heures_paris: number = 43; 
let value_heures_spectateurs_paris: number = 57; 
let value_heures_medailles_paris: number = 58; 
import { Chart, type EChartsOptions } from 'svelte-echarts'
import jo_horraire_ville from '$lib/data/jo_horraire_ville.json';
import { Accordion, AccordionItem, ListBox, ListBoxItem } from '@skeletonlabs/skeleton';
import DocsPreview from '$lib/components/DocsPreview.svelte';

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
  

  const options_spectateurs: EChartsOptions = {
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

  let showcase = 'Carte des Jeux';
</script>

<div class=' mx-4 lg:mx-12 ' >
  <br/>
  <br/>
  <h1 class="h1 ">
      <span class="bg-gradient-to-br from-secondary-900 to-secondary-600 bg-clip-text text-transparent box-decoration-clone">Les Jeux de Paris... vraiment ?</span>
  </h1>
  <br/>
  <p> Saviez-vous que <span class="em-secondary" role="presentation">seulement 58% des sessions</span>des jeux olympiques et paralympiques auront lieu à Paris ?</p>
  <p> Et si vous deviez relier chaque site de compétition, il vous faudrait parcourir <span class="em-secondary" role="presentation">37 070 km...</span> et si l'on enlève Tahiti, le voyage serait moins long mais serait quand même de 2560 km, soit {2560/42.195} marathons.
  <br/>
  <h2>Chiffres clés :</h2>
  <div class="grid grid-cols-3 gap-4  text-center items-center">
      <div class="card shadow-xl bg-secondary-50">
          <header class="card-header">Epreuves en dehors de Paris, en heures</header>
          <section class="p-4 flex items-center justify-center">
              <ProgressRadial value={value_heures_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
                  1 473 heures
              </ProgressRadial>
          </section>
      </div>
      <div class="card shadow-xl bg-secondary-50">
          <header class="card-header">Spectateurs en dehors de Paris, au total</header>
          <section class="p-4 flex items-center justify-center">
              <ProgressRadial value={value_heures_spectateurs_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
                  10,4 millions
              </ProgressRadial>
          </section>
      </div>
      <div class="card shadow-xl bg-secondary-50">
          <header class="card-header">Médailles décernées en dehors de Paris</header>
          <section class="p-4 flex items-center justify-center">
          <ProgressRadial value={value_heures_medailles_paris} stroke={60} meter="stroke-secondary-500" track="stroke-secondary-400/30" strokeLinecap="butt">
              889 médailles
          </ProgressRadial>
      </section>
      </div>
</div>


<div class="grid grid-cols-1 lg:grid-cols-3 gap-4 md:gap-10 items-start mt-4 lg:mt-8">
	<!-- Info -->
	<div class="space-y-4">
		<h3 class="h3">Pour aller plus loin :</h3>
		
		<p>
			Découvrez les villes concernées par les Jeux de <i>Paris</i> durant toute la période olympique.
		</p>
		<ListBox active="variant-filled" hover="hover:variant-soft" rounded="rounded-container-token" padding="!p-4">
			<ListBoxItem bind:group={showcase} name="showcase" value="Carte des Jeux">
				<div class="space-y-2">
					<h3 class="h3">Carte interactive</h3>
					<p class="!text-sm">Paris, la région parisienne mais aussi...</p>
				</div>
			</ListBoxItem>
			<ListBoxItem bind:group={showcase} name="showcase" value="Spectateurs par ville">
				<div class="space-y-2">
					<h3 class="h3">Graphique</h3>
					<p class="!text-sm">Spectateurs par jour et ville</p>
				</div>
			</ListBoxItem>
			
		</ListBox>
	</div>
  <div class="col-span-2">
		<DocsPreview label={showcase} regionPreview="min-h-[320px]">
			<svelte:fragment slot="preview">
				{#if showcase === 'Carte des Jeux'}
          <img src="carto.svg" alt="Carto" style="max-height: 450px; width: auto;" />
      {:else if showcase === 'Spectateurs par ville'}
        <p>Nombre de spectateurs par ville et par période</p>
        
      {/if}
			</svelte:fragment>
		</DocsPreview>
    </div>
	</div>
  
</div>
<div>
  <h3>Chart</h3>
  <Chart {options_spectateurs} />
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