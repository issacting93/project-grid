<script>
	import Menu from './Menu.svelte';
	import Section01 from './section01.svelte';
	import Section02 from './section02.svelte';
	import RadialInterface from './RadialInterface.svelte';
	import SpineSection from './SpineSection.svelte';
	
</script>

<link rel="preconnect" href="https://fonts.googleapis.com">
<svelte:head>
	<script src="https://code.jquery.com/pep/0.4.3/pep.js"></script>
</svelte:head>

<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Barlow:wght@100;500;600;900&display=swap" rel="stylesheet">
<main>
	<Menu />
	<Section01 />
	<RadialInterface />
	<Section02 />
	<SpineSection />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>