<script lang="ts">
    import { popup } from '@skeletonlabs/skeleton';
    import type { PopupSettings } from '@skeletonlabs/skeleton';
    import { Chart, type EChartsOptions } from 'svelte-echarts';
    import { writable } from 'svelte/store';
        import heatmap_data from '$lib/data/heatmap.json';
import { selectedGame } from '$lib/stores/stores';


  
    const filteredData = () => {
      const currentGame = $selectedGame; // Utilisation correcte du store
      if (currentGame === 'Jeux Olympiques') {
        return heatmap_data.filter(d => d.Jeux === 'Olympiques');
      } else if (currentGame === 'Jeux Paralympiques') {
        return heatmap_data.filter(d => d.Jeux === 'Paralympiques');
      } else {
        return heatmap_data;
      }
    };
const filteredArray = filteredData(); // appeler la fonction pour obtenir le tableau filtré

  const hours =     ['07:00', '08:00', '09:00', '10:00', '11:00', '12:00', 
    '13:00', '14:00', '15:00', '16:00', '17:00', '18:00', 
    '19:00', '20:00', '21:00', '22:00', '23:00', '00:00', '01:00', '02:00'];
// prettier-ignore

let days_set: Set<string> = new Set();

filteredArray.forEach((item: any) => {
  days_set.add(item.jour_semaine);
});
let days: string[] = Array.from(days_set).reverse();

console.log(days);  
// prettier-ignore
let data: number[][] = [];


const color_gradient: string[] = [];
const n = 11; // Nombre de couleurs à générer

for (let i = 0; i < n; i++) {
  const value = Math.round((1 - i / (n - 1)) * 255); 
  const r = value; 
  const g = value; 
  const b = 255; 
  color_gradient.push(`rgb(${r}, ${g}, ${b})`); // Ajouter la couleur au tableau
}
console.log(color_gradient)
let options: EChartsOptions;
$: {
    data=[];
    let max: number = 0;

      const currentGame = $selectedGame;
    const filteredHeatmap = heatmap_data.filter(d =>
    currentGame === 'Jeux Olympiques' ? d.Jeux === 'Olympiques' :
    currentGame === 'Jeux Paralympiques' ? d.Jeux === 'Paralympiques' :
    true
  );
  days_set.clear();
  filteredHeatmap.forEach((item: any) => {
    
    days_set.add(item.jour_semaine);
  });
  console.log("YOO",filteredHeatmap);
  let days: string[] = Array.from(days_set).reverse();
    console.log("YOO",days);

for (const day of days) {
  for (const hour of hours) {
const count = filteredHeatmap.filter(
    (item: any) => item.jour_semaine === day && item.heure_unique === hour
  ).length;

    data.push([hours.indexOf(hour), days.indexOf(day), count]);
  }
}

data.map(function (item) {
    return [item[1], item[0], item[2] || '-'];
});

max = data.reduce((maxCount: number, item: number[]) => {
  return Math.max(maxCount, item[2]);
}, 0);

   options= { 
  tooltip: {
    position: 'top'
  },
  grid: {
    height: '70%',
    top: '5%'
  },
  xAxis: {
    type: 'category',
    data: hours,
    splitArea: {
      show: true
    }
  },
  yAxis: {
    type: 'category',
    data: days,
    splitArea: {
      show: true
    }
  },
  visualMap: {
    min: 0,
    max: max,
    calculable: true,
    orient: 'vertical',
    left: '90%',
    bottom: '50%',
     inRange: {
      color: color_gradient
    }
  },
  series: [
    {
      name: 'Nombre d\'épreuves',
      type: 'heatmap',
      data: data,
      label: {
        show: false
      },
      emphasis: {
        itemStyle: {
          shadowBlur: 100,
          shadowColor: 'rgba(0, 0, 0, 0.5)'
        }
      }
    }
  ]
};
  }
   
  const selectedGame_heatmap = writable('Jeux Olympiques');
    const activités = () => {
        const uniqueDisciplines = new Set(filteredArray.map(d => d.Activité));
        return Array.from(uniqueDisciplines);
      };          
  </script>
    <div class=' mx-4 mt-4 lg:mx-12 mt-12'>

  <p class="mt-8 lg:mt-12">
          Pour aller plus loin, voilà de quoi vous aider à définir votre programme télé  :

  </p>

    
    <div class="grid grid-cols-1 gap-4 text-center">
      <div class="card shadow-xl h-96 md:h-96 mt-2">
  
        <br/>
        <div class="flex flex-col-2 justify-center space-x-4">
          <button class="btn bg-surface-300" on:click={() => ($selectedGame = "Jeux Olympiques")}>Jeux Olympiques</button>
          <button class="btn bg-surface-300" on:click={() => ($selectedGame ='Jeux Paralympiques')}>Jeux Paralympiques</button>
         <!--  <select class="btn bg-surface-300 border-0 font-bold focus:outline-none focus:bg-white focus:border-gray-500">
              <option value="" disabled selected>Filtrer selon vos préférences</option>
            {#each activités() as activité}
              <option value={activité}>{activité}</option>
            {/each}
    </select>-->
        </div>
  
        <Chart {options} />
  
      </div>
    </div>

    <br/>

      
  </div>
  

  