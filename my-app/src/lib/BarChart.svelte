<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import Chart from 'chart.js/auto';

	export let data: { date: string; count: number }[];
	export let timeRange: 'hour' | 'day' | 'week' | 'year';

	let chart: Chart;
	let canvas: HTMLCanvasElement;

	// Function to create or update the chart
	function createOrUpdateChart() {
		if (chart) {
			chart.destroy(); // Destroy the existing chart
		}

		// Group data based on the selected time range
		const groupedData = groupDataByTimeRange(data, timeRange);

		// Create the bar chart
		chart = new Chart(canvas, {
			type: 'bar',
			data: {
				labels: groupedData.labels, // Dates or time ranges on the x-axis
				datasets: [
					{
						label: 'Contributions',
						data: groupedData.counts, // Counts on the y-axis
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

	// Create the chart when the component mounts
	onMount(() => {
		createOrUpdateChart();
	});

	// Update the chart when data or timeRange changes
	$: {
		createOrUpdateChart();
	}

	// Cleanup on component destruction
	onDestroy(() => {
		if (chart) {
			chart.destroy();
		}
	});

	// Function to group data by time range
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
					key = date.toISOString().slice(0, 13); // Group by hour (YYYY-MM-DDTHH)
					break;
				case 'day':
					key = date.toISOString().slice(0, 10); // Group by day (YYYY-MM-DD)
					break;
				case 'week':
					key = getWeekKey(date); // Group by week (YYYY-WW)
					break;
				case 'year':
					key = date.toISOString().slice(0, 4); // Group by year (YYYY)
					break;
				default:
					key = date.toISOString().slice(0, 10); // Default to day
			}

			grouped[key] = (grouped[key] || 0) + entry.count;
		});

		return {
			labels: Object.keys(grouped),
			counts: Object.values(grouped)
		};
	}

	// Function to get the week key (YYYY-WW)
	function getWeekKey(date: Date): string {
		const startOfWeek = new Date(date);
		startOfWeek.setHours(0, 0, 0, 0);
		startOfWeek.setDate(date.getDate() - date.getDay()); // Start of the week (Sunday)
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
