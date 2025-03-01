<script lang="ts">
	export let title: string;
	export let selectedColor: string;
	export let contributions: any;
	let contentTime = `Just now`;

	const today = new Date().toISOString().split('T')[0];

	// Find today's contribution or initialize it
	let todayContribution = contributions.find((c: { date: string }) => c.date === today);
	if (!todayContribution) {
		todayContribution = { date: today, count: 0 };
		contributions.push(todayContribution);
	}

	// Update the count for today's contribution
	function addBtnHandleClick() {
		todayContribution.count++;
	}

	function removeBtnHandleClick() {
		if (todayContribution.count > 0) {
			todayContribution.count--;
		}
	}
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
			removeBtnHandleClick();
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
			addBtnHandleClick();
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
