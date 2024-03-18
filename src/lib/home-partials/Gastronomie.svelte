<script lang="ts">
    import { onMount } from 'svelte';
    import nb_resto_lieu from "$lib/data/bn_resto_lieu.json";
    import { createEventDispatcher } from 'svelte';
  import ScatterLieu from '$lib/home-partials/ScatterLieu.svelte';
    import Podium from '$lib/home-partials/Podium.svelte';
    import Podium2 from '$lib/home-partials/Podium2.svelte';
  import { choixLieuStore } from '$lib/stores/stores';

    const lieux = ['']; // Ajouter une option vide au début de la liste
    nb_resto_lieu.forEach(item => lieux.push(item.lieu)); // Ajouter tous les lieux du JSON
  
    const dispatch = createEventDispatcher();
  
    let selectedLieu = 'Stade tour Eiffel'; // Lieu sélectionné
    let lieuInfo = {}; // Informations sur le lieu sélectionné
  
    // Fonction pour mettre à jour les informations du lieu sélectionné
    function updateLieuInfo() {
      lieuInfo = nb_resto_lieu.find(item => item.lieu === selectedLieu) || {};
    }
  
    // Mettre à jour les informations du lieu au chargement de la page
    onMount(() => {
      updateLieuInfo();
    });
  
    // Écouter les changements de sélection dans la liste déroulante
    function handleSelectChange(event) {
      selectedLieu = event.target.value;
      updateLieuInfo();
      dispatch('lieuChanged', selectedLieu); // Dispatch un événement si le lieu change
      $choixLieuStore = selectedLieu;
    }
  </script>
  
  <div class=' mx-4 lg:mx-12'>
    <br/>
    <br/>
    <h1 class="h2">
      <span class="bg-gradient-to-br from-primary-800 to-primary-400 bg-clip-text text-transparent box-decoration-clone">La France, championne de la Gastronomie ?</span>
    </h1>
    <br />
   <p>  Les épreuves seront aussi l'occasion de découvrir la France et sa gastronomie : plus <span class="em-primary" role="presentation">30 000 restaurants</span> sont à 15min à pied des sites des épreuves !</p>
   <p>Choisissez un quartier pour savoir si vous aurez plus de chances de manger de la cuisine française traditionnelle ou de la cuisine du monde.  </p>
    <div class="grid grid-cols-4 gap-4 mt-4  text-center">
  
    <select class="select bg-primary-50" on:change={handleSelectChange} >
        <option value="">Choisir un lieu</option>
        {#each lieux as lieu}
          {#if lieu === "Stade tour Eiffel"} 
                      <option selected value={lieu}>{lieu}</option>
          {:else}
            <option value={lieu}>{lieu}</option>
           {/if}
        {/each}
      </select>
  
      <div class="card shadow-xl bg-primary-50">
        <header class="card-header">Capacité</header>
        <section class="p-4 flex items-center justify-center">
         <b> {selectedLieu && lieuInfo.capacite ? lieuInfo.capacite : ''}</b>
        </section>
      </div>
  
      <div class="card shadow-xl bg-primary-50">
        <header class="card-header">Nombre de restaurants</header>
        <section class="p-4 flex items-center justify-center">
            <b>{selectedLieu && lieuInfo.n ? lieuInfo.n : ''}</b>
        </section>
      </div>
  
      <div class="card shadow-xl bg-primary-50">
        <header class="card-header">Restaurant le plus proche</header>
        <section class="p-4 flex items-center justify-center">
            <b>{selectedLieu && lieuInfo.nom_plus_pres && lieuInfo.plus_pres ? `${lieuInfo.nom_plus_pres} - ${lieuInfo.plus_pres}m` : ''}</b>
        </section>
      </div>
      
    </div>

    <div class="grid grid-cols-1 gap-4 mt-4 text-center min-h-screen-80 ">
      <div class="card min-h-screen-80 shadow-xl bg-primary-50 h-full md:h-full mt-2">
        <div class="w-full min-h-screen-80 h-full flex justify-center items-center">

         <ScatterLieu />
         </div>
      </div>
      </div>
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mt-4 text-center">
      <div class="card shadow-xl bg-primary-50 h-full md:h-full mt-2">
        <div class="w-full min-h-screen-80  h-full flex justify-center items-center">
<Podium />
           </div>
         
      </div>
      <div class="card shadow-xl bg-primary-50 h-full md:h-full mt-2">
        <div class="w-full min-h-screen-80  h-full flex justify-center items-center">

         <Podium2 />
         </div>
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
    .scatter-lieu {
    width: 100%;
    min-height: 80vh; /* définir une hauteur minimale de 80% de la hauteur de l'écran */
  }
</style>