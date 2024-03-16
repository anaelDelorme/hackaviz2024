<script lang="ts">
	import '../app.postcss';
	import { AppShell, AppBar, popup } from '@skeletonlabs/skeleton';
	import type { SubmitFunction } from '@sveltejs/kit';
	import { storeTheme } from '$lib/stores/stores';
	import { onMount } from 'svelte';
	// Highlight JS
	import hljs from 'highlight.js/lib/core';
	import 'highlight.js/styles/github-dark.css';
	import { storeHighlightJs } from '@skeletonlabs/skeleton';
	import xml from 'highlight.js/lib/languages/xml'; // for HTML
	import css from 'highlight.js/lib/languages/css';
	import javascript from 'highlight.js/lib/languages/javascript';
	import typescript from 'highlight.js/lib/languages/typescript';

	hljs.registerLanguage('xml', xml); // for HTML
	hljs.registerLanguage('css', css);
	hljs.registerLanguage('javascript', javascript);
	hljs.registerLanguage('typescript', typescript);
	storeHighlightJs.set(hljs);

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });

	//Dark or Ligth
	import { LightSwitch } from '@skeletonlabs/skeleton';

	//Choose theme
const themes = [
		{ type: 'skeleton', name: 'Skeleton', icon: '💀' },
		{ type: 'wintry', name: 'Wintry', icon: '🌨️' },
		{ type: 'modern', name: 'Modern', icon: '🤖' },
		{ type: 'rocket', name: 'Rocket', icon: '🚀' },
		{ type: 'seafoam', name: 'Seafoam', icon: '🧜‍♀️' },
		{ type: 'vintage', name: 'Vintage', icon: '📺' },
		{ type: 'sahara', name: 'Sahara', icon: '🏜️' },
		{ type: 'hamlindigo', name: 'Hamlindigo', icon: '👔' },
		{ type: 'gold-nouveau', name: 'Gold Nouveau', icon: '💫' },
		{ type: 'crimson', name: 'Crimson', icon: '⭕' }
		// { type: 'seasonal', name: 'Seasonal', icon: '🎆' }
		// { type: 'test', name: 'Test', icon: '🚧' },
	];

	let selectedTheme = '';
  let isOpen = false;

  onMount(() => {
    const currentTheme = document.body.getAttribute('data-theme');
    selectedTheme = currentTheme || themes[0].type;
  });

  const toggleList = () => {
    isOpen = !isOpen;
  };

  const handleThemeChange = (event) => {
    const theme = event.target.value;

    if (theme) {
      document.body.setAttribute('data-theme', theme);
      $storeTheme = theme;
      isOpen = false;
    }
  };

  const handleOutsideClick = (event) => {
    if (!event.target.closest('.theme-picker')) {
      isOpen = false;
    }
  };

  onMount(() => {
    window.addEventListener('click', handleOutsideClick);

    return () => {
      window.removeEventListener('click', handleOutsideClick);
    };
  });
</script>
<svelte:head>
	<title>Hackaviz 2024</title>
	<meta name="description" content="hackaviz 2024 sur les jeux de Paris." />
	
</svelte:head>
<!-- App Shell -->
<AppShell>
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<strong class="text-xl uppercase">Hackaviz 2024</strong>
				<img src="olympics.svg" alt="Olympics" class="ml-2 w-10 h-10" />
			</svelte:fragment>
			<svelte:fragment slot="trail" >
				<div class="theme-picker">
				
					<button class="btn hover:variant-soft-primary" use:popup={{ event: 'click', target: 'theme', closeQuery: 'a[href]' }}>
						<i class="fa-solid fa-palette text-lg md:!hidden" />
						<img src="theme.png" alt="Theme Icon" class="w-6 h-6" /><span class="hidden md:inline-block">Changer de thème</span>
						<i class="fa-solid fa-caret-down opacity-50" />
					</button>
					<div class="card p-4 w-60 shadow-xl" data-popup="theme" >
						<div class="space-y-4">
							<section class="flex justify-between items-center">
								<h6 class="h6">Mode</h6>
								<LightSwitch />
							</section>
							<hr />
							<nav class="list-nav p-4 -m-4 max-h-64 lg:max-h-[500px] overflow-y-auto">
								<ul>
								  {#each themes as { type, name, icon }}
									<li>
									  <button on:click={() => handleThemeChange({ target: { value: type } })}>
										{icon} {name}
									  </button>
									</li>
								  {/each}
								</ul>
							</nav>
						</div>
				  </div>
				</div>
				
				<a
					
					href="https://github.com/anaelDelorme/hackaviz2024"
					target="_blank"
					rel="noreferrer"
				>
				<img src="github.png" alt="Github Icon" class="w-6 h-6" />
				</a>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<!-- Page Route Content -->
	<slot />
	<svelte:fragment slot="pageFooter">

	<footer class="bg-white rounded-lg shadow m-4 dark:bg-gray-800">
		<div class="w-full mx-auto max-w-screen-xl p-4 md:flex md:items-center md:justify-between">
		  <span class="text-sm text-gray-500 sm:text-center dark:text-gray-400">© 2024 <a href="https://flowbite.com/" class="hover:underline">Les Datathlètes</a>
		</span>
		<ul class="flex flex-wrap items-center mt-3 text-sm font-medium text-gray-500 dark:text-gray-400 sm:mt-0">
	
			<li>
				<a href="#" class="hover:underline">Anaël Delorme</a>
			</li>
			<li>
				<span class="mx-2">&nbsp;</span>

				<a href="#" class="hover:underline">François Semecurbe</a>
			</li>
			<li>
				<span class="mx-2">&nbsp;</span>

				<a href="#" class="hover:underline">Laurette Cretin</a>
			</li>
			<li>
				<span class="mx-2">&nbsp;</span>

				<a href="#" class="hover:underline">Marion Roué</a>
			</li>
			<li>

			<img src="olympics.svg" alt="Olympics" class="ml-2 w-10 h-10" />
		</li>
		</ul>
		</div>
	</footer>
	</svelte:fragment>
</AppShell>
