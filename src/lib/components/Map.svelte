<script lang="ts">
	import { Map } from 'maplibre-gl';
	import { onMount, setContext } from 'svelte';

	let map: Map;
	setContext('map', () => map);

	onMount(() => {
		map = new Map({
			container: 'map',
			style:
				'https://api.maptiler.com/maps/streets/style.json?key=get_your_own_OpIi9ZULNHzrESv6T2vL',
			center: [0, 0],
			zoom: 2
		});
	});
</script>

<div id="map"></div>

<slot />

<style>
	#map {
		width: 100%;
		height: 100%;
	}
</style>
