<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import Chart from 'chart.js/auto';

	export let data: { date: string; count: number }[];
	export let timeRange: 'hour' | 'day' | 'week' | 'year';

	let chart: Chart;
	let canvas: HTMLCanvasElement;

	function createOrUpdateChart() {
		if (chart) {
			chart.destroy();
		}

		const groupedData = groupDataByTimeRange(data, timeRange);

		chart = new Chart(canvas, {
			type: 'bar',
			data: {
				labels: groupedData.labels,
				datasets: [
					{
						label: 'Contributions',
						data: groupedData.counts,
						backgroundColor: 'rgba(75, 192, 192, 0.2)',
						borderColor: 'rgba(75, 192, 192, 1)',
						borderWidth: 1
					}
				]
			},
			options: {
				responsive: true,
				scales: {
					y: {
						beginAtZero: true,
						title: {
							display: true,
							text: 'Contributions'
						}
					},
					x: {
						title: {
							display: true,
							text: 'Time'
						}
					}
				}
			}
		});
	}

	onMount(() => {
		createOrUpdateChart();
	});

	$: {
		createOrUpdateChart();
	}

	onDestroy(() => {
		if (chart) {
			chart.destroy();
		}
	});

	function groupDataByTimeRange(
		data: { date: string; count: number }[],
		range: 'hour' | 'day' | 'week' | 'year'
	) {
		const grouped: { [key: string]: number } = {};

		data.forEach((entry) => {
			const date = new Date(entry.date);
			let key: string;

			switch (range) {
				case 'hour':
					key = date.toISOString().slice(0, 13);
					break;
				case 'day':
					key = date.toISOString().slice(0, 10);
					break;
				case 'week':
					key = getWeekKey(date);
					break;
				case 'year':
					key = date.toISOString().slice(0, 4);
					break;
				default:
					key = date.toISOString().slice(0, 10);
			}

			grouped[key] = (grouped[key] || 0) + entry.count;
		});

		return {
			labels: Object.keys(grouped),
			counts: Object.values(grouped)
		};
	}

	function getWeekKey(date: Date): string {
		const startOfWeek = new Date(date);
		startOfWeek.setHours(0, 0, 0, 0);
		startOfWeek.setDate(date.getDate() - date.getDay());
		return `${startOfWeek.getFullYear()}-W${String(Math.ceil((startOfWeek.getDate() + 1) / 7)).padStart(2, '0')}`;
	}
</script>

<canvas bind:this={canvas}></canvas>

<style>
	canvas {
		width: 100%;
		height: 300px;
	}
</style>
