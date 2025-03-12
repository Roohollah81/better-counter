<script lang="ts">
	// @ts-nocheck
	import ActivityCalendarWidget from 'activity-calendar-widget/svelte';
	import BarChart from './BarChart.svelte';

	export let selectedItem;

	let timeRange: 'hour' | 'day' | 'week' | 'year' = 'day';

	$: data = selectedItem ? selectedItem.contributions : [];

	$: transformedData = selectedItem
		? selectedItem.contributions.map((contribution) => ({
				date: contribution.date,
				activities: Array(contribution.count).fill({})
			}))
		: [];
</script>

<div class="timeline-container">
	<div class="timeline">
		{#if selectedItem}
			<ActivityCalendarWidget daysToRender={365} data={transformedData} />
		{:else}
			<p>No item selected.</p>
		{/if}
	</div>
</div>

<div class="bar-chart-container">
	<h3>Contribution Bar Chart</h3>
	<select bind:value={timeRange}>
		<option value="hour">Per Hour</option>
		<option value="day">Per Day</option>
		<option value="week">Per Week</option>
		<option value="year">Per Year</option>
	</select>
	<BarChart {data} {timeRange} />
</div>

<style>
	.timeline-container {
		overflow-y: hidden;
		background-color: #1c293d;
	}
	.timeline {
		width: fit-content;
		min-width: 100%;
		height: 100%;
	}

	@media (max-width: 768px) {
		.timeline-container {
			padding: 8px;
		}

		.timeline {
			width: 100%;
			min-width: auto;
		}
	}

	.bar-chart-container {
		margin-top: 20px;
		background-color: #2c3e50;
		padding: 16px;
		border-radius: 6px;
	}

	h3 {
		color: white;
		margin-bottom: 10px;
	}

	select {
		margin-bottom: 10px;
		padding: 5px;
		border-radius: 5px;
		background-color: #1c293d;
		color: white;
		border: 1px solid #ccc;
	}
</style>
