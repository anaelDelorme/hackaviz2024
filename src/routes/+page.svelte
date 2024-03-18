<script lang="ts">
  import HomeHero from '$lib/home-partials/HomeHero.svelte';
  import SpectateurLieu from '$lib/home-partials/SpectateurLieu.svelte';
  import Gastronomie from '$lib/home-partials/Gastronomie.svelte';
  import ProgrammeTele from '$lib/home-partials/ProgrammeTele.svelte';
  import Heatmap_tele from '$lib/home-partials/Heatmap_tele.svelte';

</script>

<header id="hero" class="bg-surface-100-800-token hero-gradient">
	<HomeHero />
</header>

<section id="spectateur_lieu" class="bg-surface-100-800-token section1-gradient">
	<SpectateurLieu />
</section>

<section id="Gastronomie" class="bg-surface-100-800-token section2-gradient">
	<Gastronomie />
</section>

<section id="ProgrammeTele" class="bg-surface-100-800-token section3-gradient">
	<ProgrammeTele />
	<Heatmap_tele />
</section>

<style lang="postcss">
      .em-primary {
          padding: 1px 4px 1px 4px;
          font-weight: bold;
		  white-space: nowrap;
		  background-color: var(--primary-500);
  			color: white;
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
	.section-container {
		@apply w-full max-w-7xl mx-auto p-4 py-16 md:py-24;
	}
	.hero-gradient {
		background-image:
			radial-gradient(at 0% 0%, rgba(var(--color-primary-500) / 0.33) 0px, transparent 50%),
			radial-gradient(at 98% 1%, rgba(var(--color-secondary-500) / 0.33) 0px, transparent 50%);
	}
	.section1-gradient {
		background-image:
			radial-gradient(at 0% 100%, rgba(var(--color-secondary-500) / 0.50) 0px, transparent 50%);
	}
	.section2-gradient {
    background-image:
        radial-gradient(at 0% 0%, rgba(var(--color-secondary-500) / 0.50) 0px, transparent 50%),
        radial-gradient(at 100% 100%, rgba(var(--color-primary-500) / 0.24) 0px, transparent 50%);
}

.section3-gradient {
    background-image:
        radial-gradient(at 0% 100%, rgba(var(--color-primary-500) / 0.50) 0px, transparent 50%),
        radial-gradient(at 100% 0%, rgba(var(--color-primary-500) / 0.24) 0px, transparent 50%);
}

</style>
