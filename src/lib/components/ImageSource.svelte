<script lang="ts">
	import { browser } from '$app/environment';
	import type { Map } from 'maplibre-gl';
	import { getContext, onMount } from 'svelte';

	export let topLeft: [number, number];
	export let topRight: [number, number];
	export let bottomRight: [number, number];
	export let bottomLeft: [number, number];

	export let mapOpacity = 0.5;

	let hasInit = false;
	let map: Map;
	const getMap: () => Map = getContext('map');

	$: {
		if (browser && map !== undefined) {
			let mySource = map.getSource('map-image');
			mySource?.setCoordinates([topLeft, topRight, bottomRight, bottomLeft]);
		}
	}

	$: {
		if (browser && map !== undefined) {
			if (!hasInit) hasInit = true;
			else map.setPaintProperty('map-image-layer', 'raster-opacity', mapOpacity);
		}
	}

	onMount(() => {
		setTimeout(() => {
			map = getMap();

			map.on('load', () => {
				map.addSource('map-image', {
					type: 'image',
					url: 'map.png',
					coordinates: [topLeft, topRight, bottomRight, bottomLeft]
				});

				map.addLayer({
					id: 'map-image-layer',
					type: 'raster',
					source: 'map-image',
					paint: {
						'raster-fade-duration': 0
					}
				});

				map.setPaintProperty('map-image-layer', 'raster-opacity', mapOpacity);
			});
		});
	});
</script>
