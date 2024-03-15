<script lang="ts">
    import { onMount } from 'svelte';
    import nb_resto_lieu from "$lib/data/bn_resto_lieu.json";
    import { createEventDispatcher } from 'svelte';
  
    const lieux = ['']; // Ajouter une option vide au début de la liste
    nb_resto_lieu.forEach(item => lieux.push(item.lieu)); // Ajouter tous les lieux du JSON
  
    const dispatch = createEventDispatcher();
  
    let selectedLieu = ''; // Lieu sélectionné
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
    }
  </script>
  
  <div class=' mx-4 lg:mx-12'>
    <br/>
    <br/>
    <h1 class="h1">
      <span class="bg-gradient-to-br from-primary-800 to-primary-400 bg-clip-text text-transparent box-decoration-clone">La France, capitale de la Gastronomie ?</span>
    </h1>
    <br />
    <div class="grid grid-cols-4 gap-4  text-center">
  
      <select class="select bg-primary-50" on:change={handleSelectChange}>
        <option value="">Choisir un lieu</option>
        {#each lieux as lieu}
          <option value={lieu}>{lieu}</option>
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
  </div>
  