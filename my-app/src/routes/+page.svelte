<!-- src/routes/+page.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';
	import Row from '../lib/Row.svelte';
	import { fly } from 'svelte/transition';

	let selectedTitle = '';
	let showSidebar = false;

	function handleClickOnRowContent(title: string) {
		selectedTitle = title;
		showSidebar = true;
	}

	const items = [
		{ title: 'Title name 1', count: 13, timestamp: new Date() },
		{ title: 'Title name 2', count: 3, timestamp: new Date() },
		{ title: 'Title name 3', count: 80, timestamp: new Date() },
		{ title: 'Title name 4', count: 6, timestamp: new Date() },
		{ title: 'Title name 5', count: 9, timestamp: new Date() },
		{ title: 'Title name 6', count: 8, timestamp: new Date() },
		{ title: 'Title name 7', count: 7, timestamp: new Date() },
		{ title: 'Title name 8', count: 7, timestamp: new Date() },
		{ title: 'Title name 9', count: 7, timestamp: new Date() },
		{ title: 'Title name 10', count: 7, timestamp: new Date() },
		{ title: 'Title name 11', count: 7, timestamp: new Date() },
		{ title: 'Title name 12', count: 7, timestamp: new Date() },
		{ title: 'Title name 13', count: 7, timestamp: new Date() }
	];
</script>

<div class="rows">
	<div class="head">
		<h3 class="head-title">Better Counter</h3>
		<label class="show-sidebar-checkbox">
			<input type="checkbox" bind:checked={showSidebar} />
			Chart
		</label>
	</div>
	{#each items as item}
		<Row
			on:click={() => handleClickOnRowContent(item.title)}
			title={item.title}
			count={item.count}
			timestamp={item.timestamp}
		></Row>
	{/each}
</div>
{#if showSidebar}
	<div class="sidebar" transition:fly={{ y: 200, duration: 200 }}>
		<h3 class="sidebar-title" style="bold">{selectedTitle}</h3>
	</div>
{/if}

<style>
	.head {
		width: 100%;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: var(--items-color);
	}

	.head-title {
		margin-left: 20px;
	}

	.show-sidebar-checkbox {
		margin-right: 10px;
	}

	.rows {
		width: 100%;
		color: white;
		display: flex;
		gap: var(--items-gap);
		flex-direction: column;
	}

	.sidebar {
		left: 0;
		right: 0;
		bottom: 0;
		color: white;
		height: 400px;
		position: fixed;
		animation: all 0.2s;
		background-color: #414141;
	}

	.sidebar-title {
		margin: 10px;
	}
</style>
