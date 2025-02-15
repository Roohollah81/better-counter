<script lang="ts">
	export let title: string;
	export let count: number;
	export let timestamp: number;
</script>

<link
	rel="stylesheet"
	href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0&icon_names=add"
/>
<link
	rel="stylesheet"
	href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0&icon_names=remove"
/>
<div class="item">
	<button class="btn">
		<span class="material-symbols-outlined"> remove </span>
	</button>
	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<div class="content" on:click>
		<div class="title">{title}</div>
		<div class="title">{count}</div>
		<div class="title">{timestamp}</div>
	</div>
	<button class="btn">
		<span class="material-symbols-outlined"> add </span>
	</button>
</div>

<style>
	.item {
		display: flex;
		flex-direction: row;
		gap: var(--items-gap);
		line-height: 2em;
	}
	.btn {
		display: grid;
		width: 20%;
		background-color: var(--items-color);
		border: none;
		align-items: center;
	}

	.material-symbols-outlined {
		font-size: 25px;
		color: white;
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
