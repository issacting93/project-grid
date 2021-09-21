<script>
	import Menu from './Menu.svelte';
	import Section01 from './section01.svelte';
	import Section02 from './section02.svelte';
	import RadialInterface from './RadialInterface.svelte';
</script>

<main>
	<Menu />
	<Section01 />
	<RadialInterface />
	<Section02 />
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