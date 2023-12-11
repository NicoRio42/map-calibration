<script lang="ts">
	import type { LngLat, Map, MapMouseEvent } from 'maplibre-gl';
	import { getContext, onMount } from 'svelte';

	export let coords: [number, number];
	export let color = 'blue';

	let map: Map;
	const getMap: () => Map = getContext('map');

	onMount(() => {
		setTimeout(() => {
			map = getMap();
			const sourceName = `point-${Math.random()}`;
			const layerName = `point-${Math.random()}`;

			const canvas = map.getCanvasContainer();

			const geojson = {
				type: 'FeatureCollection',
				features: [
					{
						type: 'Feature',
						geometry: {
							type: 'Point',
							coordinates: coords
						}
					}
				]
			};

			function onMove(e: MapMouseEvent) {
				coords = [e.lngLat.lng, e.lngLat.lat];

				// Set a UI indicator for dragging.
				canvas.style.cursor = 'grabbing';

				// Update the Point feature in `geojson` coordinates
				// and call setData to the source layer `point` on it.
				geojson.features[0].geometry.coordinates = coords;
				map.getSource(sourceName)?.setData(geojson);
			}

			function onUp(e: MapMouseEvent) {
				coords = [e.lngLat.lng, e.lngLat.lat];
				canvas.style.cursor = '';

				// Unbind mouse/touch events
				map.off('mousemove', onMove);
				map.off('touchmove', onMove);
			}

			map.on('load', () => {
				map.addSource(sourceName, {
					type: 'geojson',
					data: geojson
				});

				map.addLayer({
					id: layerName,
					type: 'circle',
					source: sourceName,
					paint: {
						'circle-radius': 10,
						'circle-color': color
					}
				});

				// When the cursor enters a feature in the point layer, prepare for dragging.
				map.on('mouseenter', layerName, () => {
					map.setPaintProperty(layerName, 'circle-color', '#3bb2d0');
					canvas.style.cursor = 'move';
				});

				map.on('mouseleave', layerName, () => {
					map.setPaintProperty(layerName, 'circle-color', color);
					canvas.style.cursor = '';
				});

				map.on('mousedown', layerName, (e) => {
					// Prevent the default map drag behavior.
					e.preventDefault();

					canvas.style.cursor = 'grab';

					map.on('mousemove', onMove);
					map.once('mouseup', onUp);
				});

				map.on('touchstart', layerName, (e) => {
					if (e.points.length !== 1) return;

					// Prevent the default map drag behavior.
					e.preventDefault();

					map.on('touchmove', onMove);
					map.once('touchend', onUp);
				});
			});
		});
	});
</script>
