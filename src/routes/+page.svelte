<script lang="ts">
	import DraggablePoint from '$lib/components/DraggablePoint.svelte';
	import Map from '$lib/components/Map.svelte';
	import 'maplibre-gl/dist/maplibre-gl.css';
	import './global.css';
	import ImageSource from '$lib/components/ImageSource.svelte';

	let topLeft: [number, number] = [0, 30];
	let topRight: [number, number] = [30, 30];
	let bottomRight: [number, number] = [30, 0];
	let bottomLeft: [number, number] = [0, 0];

	let mapOpacity = 0.5;
</script>

<div class="wrapper">
	<aside>
		<h1>Map calibration</h1>

		<form>
			<label>
				Import a map file

				<input type="file" placeholder="Toto" />
			</label>

			<label>
				Map opacity

				<input
					bind:value={mapOpacity}
					type="range"
					min="0"
					max="1"
					step="0.01"
					class="map-opacity-input"
				/>
			</label>
		</form>

		<table>
			<thead>
				<tr>
					<th></th>
					<th>x</th>
					<th>y</th>
					<th>long</th>
					<th>lat</th>
				</tr>
			</thead>

			<tbody>
				<tr>
					<td>Top Left</td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
				</tr>
				<tr>
					<td>Top Right</td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
				</tr>
				<tr>
					<td>Bottom Right</td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
				</tr>
				<tr>
					<td>Bottom Left</td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
					<td><input type="number" class="coord-input" /></td>
				</tr>
			</tbody>
		</table>
	</aside>

	<main>
		<input type="search" class="location-input" placeholder="Go to location" />

		<Map>
			<ImageSource {topLeft} {topRight} {bottomRight} {bottomLeft} {mapOpacity}></ImageSource>

			<DraggablePoint bind:coords={topLeft} color="red"></DraggablePoint>
			<DraggablePoint bind:coords={topRight} color="green"></DraggablePoint>
			<DraggablePoint bind:coords={bottomRight} color="blue"></DraggablePoint>
			<DraggablePoint bind:coords={bottomLeft} color="yellow"></DraggablePoint>
		</Map>
	</main>
</div>

<style>
	.wrapper {
		display: grid;
		min-width: 0;
		min-height: 0;
		height: 100%;
		width: 100%;
		grid-template-columns: 1fr 1fr;
	}

	aside {
		padding: 0.5rem;
	}

	h1 {
		margin-bottom: 1rem;
	}

	.map-opacity-input {
		margin-bottom: 0;
	}

	main {
		min-width: 0;
		min-height: 0;
		flex-grow: 1;
		position: relative;
	}

	.location-input {
		position: absolute;
		top: 0.5rem;
		left: 0.5rem;
		right: 0.5rem;
		z-index: 1;
		width: auto;
	}
</style>
