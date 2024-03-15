<script lang="ts">
	import '../app.postcss';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';

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
			<svelte:fragment slot="trail">
				<LightSwitch />
				<a
					class="btn btn-sm variant-ghost-surface"
					href="https://github.com/anaelDelorme/hackaviz2024"
					target="_blank"
					rel="noreferrer"
				>
					GitHub
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
