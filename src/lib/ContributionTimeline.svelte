<script lang="ts">
	// @ts-nocheck
	import ActivityCalendarWidget from 'activity-calendar-widget/svelte';
	import BarChart from './BarChart.svelte';

	export let transformedData;
</script>

<div class="timeline-container">
	<div class="timeline">
		{#if transformedData}
			<ActivityCalendarWidget daysToRender={365} data={transformedData} />
		{:else}
			<p>No item selected.</p>
		{/if}
	</div>
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
</style>
