<script lang="ts">
    import { popup } from '@skeletonlabs/skeleton';
    import type { PopupSettings } from '@skeletonlabs/skeleton';
    import { Chart, type EChartsOptions } from 'svelte-echarts';
    import { writable } from 'svelte/store';
    import prime_time from '$lib/data/prime_time.json';
import { selectedGame } from '$lib/stores/stores';

  console.log(selectedGame);

    
    const popupFeatured: PopupSettings = {
      event: 'hover',
      target: 'popupFeatured',
      placement: 'bottom',
    };
  
    let options: EChartsOptions;

    $: {
      const currentGame = $selectedGame;
      const prime_timeSansCerem = prime_time.filter(d =>d.discipline_regroup!="Cérémonie")
    const filteredPrimeTime = prime_timeSansCerem.filter(d =>
    currentGame === 'Jeux Olympiques' ? d.jeux === 'Olympiques' :
    currentGame === 'Jeux Paralympiques' ? d.jeux === 'Paralympiques' :
    true
  );
        
      options = {
        xAxis: {
          type: 'value',
        },
        yAxis: {
          type: 'category',
          data: filteredPrimeTime.map(d => d.discipline_regroup),
        },
        series: [
          {
            data: filteredPrimeTime.map(d => d.part_prime_time),
            type: 'bar',
            itemStyle: {
              color: "#3f4088"
            }
          },
        ],
        grid: {
          left: '3%',
          right: '3%',
          bottom: '27%',
          containLabel: true,
          height: "70%"
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          },
          formatter: function (params) {
      const value = params[0].value; // obtenir la valeur
      const percentage = Math.round(value) + ' %'; // convertir en pourcentage et arrondir à l'unité
      return `<p>${params[0].name}</p>
              <p>${percentage}</p>`; // retourner le tooltip avec le nom et le pourcentage
    }
        }
      };
    }
    const hours = ['07:00', '08:00', '09:00', '10:00', '11:00',
        '12:00', '13:00', '14:00', '15:00', '16:00', '17:00',
        '18:00', '19:00', '20:00', '21:00', '22:00', '23:00', 
        '00:00', '01:00', '02:00'];

    
  </script>
  
  <div class=' mx-4 lg:mx-12'>
    <br/>
    <br/>
    <h1 class="h2">
      <span class="bg-gradient-to-br from-surface-900 to-surface-400 bg-clip-text text-transparent box-decoration-clone">Préparez votre programme télé !</span>
    </h1>
    <br/>
    <p>
      Toutes les disciplines n’auront pas la même visibilité. Si l’athlétisme est la championne des sessions en <a href="#" use:popup={popupFeatured}>prime-time</a>, certaines disciplines sont plutôt programmées en journée.
    </p>
    <div class="grid grid-cols-1 gap-4 text-center">
      <div class="card shadow-xl h-96 md:h-96 mt-8">
        
        <br/>
        <div class="flex justify-center space-x-4 mb-2">
          <button class="btn bg-surface-300" on:click={() => ($selectedGame = "Jeux Olympiques")}>Jeux Olympiques</button>
          <button class="btn bg-surface-300" on:click={() => ($selectedGame ='Jeux Paralympiques')}>Jeux Paralympiques</button>
        </div>
        <p>Part de sessions en prime time par discipline</p>
        <Chart {options} />
  
      </div>
    </div>
  </div>
  <div class="card p-4 w-90 shadow-xl" data-popup="popupFeatured">
    <div><p>Sessions qui se terminent après 20h.</p></div>
    <div class="arrow bg-surface-100-800-token" />
  </div>

  