<script>
	import Menu from './Menu.svelte';
	import Section01 from './section01.svelte';
	import Section02 from './section02.svelte';
	import About from './About.svelte';
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
	<About/>
	<Section02 />
	<SpineSection />

</main>

<style>
 
</style>