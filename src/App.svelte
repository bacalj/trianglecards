<script>
	export let name;
	import TriangleCards from './trianglecards.svelte';
</script>

<main>
	<h1>{name}</h1>
	<TriangleCards/>
</main>

<style>
	h1 {
		text-align: center;
	}
</style>