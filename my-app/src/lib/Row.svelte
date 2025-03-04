<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let title: string;
	export let selectedColor: string;
	export let contributions: any;
	export let latestContributionTimestamp: string; // Add this prop

	let contentTime = `Just now`;

	const today = new Date().toISOString().split('T')[0];

	// Find today's contribution or initialize it
	let todayContribution = contributions.find((c: { date: string }) => c.date === today);
	if (!todayContribution) {
		todayContribution = { date: today, count: 0, timestamp: new Date().toISOString() };
		contributions.push(todayContribution);
	}

	export function updateTime() {
		const now = new Date();

		const reference = new Date(latestContributionTimestamp); // Use the latest contribution timestamp

		const diffInSeconds = Math.floor((now.getTime() - reference.getTime()) / 1000);

		if (diffInSeconds < 0) {
			throw new Error('Reference time is in the future.');
		}

		// Convert seconds to days, hours, minutes, and seconds
		const days = Math.floor(diffInSeconds / (3600 * 24));
		const hours = Math.floor((diffInSeconds % (3600 * 24)) / 3600);
		const minutes = Math.floor((diffInSeconds % 3600) / 60);
		// const seconds = diffInSeconds % 60

		// Format the time difference
		if (diffInSeconds < 60) {
			contentTime = `Just now`;
		} else {
			contentTime = '';
			if (days) {
				contentTime += `${days}d `;
			}
			if (hours) {
				contentTime += `${hours}h `;
			}
			if (minutes) {
				contentTime += `${minutes}m `;
			}
			contentTime += 'ago';
		}
	}

	// Update the time display every minute
	setInterval(() => {
		updateTime();
	}, 60 * 1000);

	// Update the count for today's contribution
	function addBtnHandleClick() {
		todayContribution.count++;
		todayContribution.timestamp = new Date().toISOString(); // Update today's timestamp
		latestContributionTimestamp = todayContribution.timestamp; // Update the latest contribution timestamp
		dispatch('update'); // Emit an event
	}

	function removeBtnHandleClick() {
		if (todayContribution.count > 0) {
			todayContribution.count--;
			todayContribution.timestamp = new Date().toISOString(); // Update today's timestamp
			latestContributionTimestamp = todayContribution.timestamp; // Update the latest contribution timestamp
			dispatch('update'); // Emit an event
		}
	}

	// Initialize the time display
	updateTime();
</script>

<link
	rel="stylesheet"
	href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0"
/>
<div class="item">
	<button
		class="btn"
		style="background-color: {selectedColor};"
		on:click={() => {
			removeBtnHandleClick(), updateTime();
		}}
	>
		<span class="material-symbols-outlined"> remove </span>
	</button>
	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div class="content" style="background-color: {selectedColor};" on:click>
		<div class="title">{title}</div>
		<div class="count">{todayContribution.count}</div>
		<div class="timestamp">
			<span class="material-symbols-outlined timeIcon">schedule</span>
			{contentTime}
		</div>
	</div>
	<button
		class="btn"
		style="background-color: {selectedColor};"
		on:click={() => {
			addBtnHandleClick(), updateTime();
		}}
	>
		<span class="material-symbols-outlined"> add </span>
	</button>
</div>

<style>
	.item {
		display: flex;
		line-height: 2em;
		flex-direction: row;
		gap: var(--items-gap);
	}
	.btn {
		width: 20%;
		border: none;
		display: grid;
		cursor: pointer;
		align-items: center;
		background-color: var(--items-color);
	}
	.count {
		font-size: 20px;
	}
	.timestamp {
		display: flex;
		align-items: center;
	}
	.timeIcon {
		margin-right: 5px;
	}
	.material-symbols-outlined {
		color: white;
		font-size: 25px;
	}
	.content {
		display: flex;
		align-items: center;
		flex-direction: column;
		width: -webkit-fill-available;
		justify-content: space-evenly;
		background-color: var(--items-color);
	}
</style>
