<script lang="ts">
	// @ts-nocheck
	import ActivityCalendarWidget from 'activity-calendar-widget/svelte';
	import BarChart from './BarChart.svelte'; // Import the BarChart component
	
	// Declare the selectedItem prop
	export let selectedItem;

	// Time range for the bar chart
	let timeRange: 'hour' | 'day' | 'week' | 'year';

	// Transform contributions data for the bar chart
	$: data = selectedItem ? selectedItem.contributions : [];

	// Reactive transformation of the selected item's data
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

<!-- Add the bar chart at the bottom -->
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
	/* Base styles */
	.timeline-container {
		overflow-y: hidden;
		background-color: #1c293d;
	}
	.timeline {
		width: fit-content; /* Allow the timeline to grow as needed */
		min-width: 100%; /* Ensure it takes at least the full width of the container */
		height: 100%; /* Fill the container height */
	}
	/* Mobile styles */
	@media (max-width: 768px) {
		.timeline-container {
			padding: 8px; /* Reduce padding for smaller screens */
		}

		/* Adjust the timeline for mobile */
		.timeline {
			width: 100%; /* Take full width on mobile */
			min-width: auto; /* Allow it to shrink */
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
