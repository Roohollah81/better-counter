<script>
	// @ts-nocheck
	import ActivityCalendarWidget from 'activity-calendar-widget/svelte';
</script>

<div class="timeline-container">
	<div class="timeline">
		<ActivityCalendarWidget daysToRender={365} />
	</div>
</div>

<style>
	/* Base styles */
	.timeline-container {
		width: 100%; /* Full width of the parent container */
		height: 400px; /* Fixed height for the container */
		max-width: 100%; /* Ensure it doesn't exceed the page width */
		overflow-x: auto; /* Enable horizontal scrolling */
		overflow-y: auto; /* Enable vertical scrolling if needed */
		padding: 16px;
		background-color: #1c293d; /* Dark background (optional) */
		border-radius: 6px; /* Rounded corners (optional) */
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
</style>
