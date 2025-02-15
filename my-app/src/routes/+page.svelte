<!-- src/routes/+page.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';
	import Row from '../lib/Row.svelte';
	import { fly } from 'svelte/transition';

	let selectedTitle = '';

	function handleClick(title: string) {
		selectedTitle = title;
	}

	const items = [
		{ title: 'Title name 1', count: 13, timestamp: 1739548594 },
		{ title: 'Title name 2', count: 3, timestamp: 1739548594 },
		{ title: 'Title name 3', count: 80, timestamp: 1739548594 },
		{ title: 'Title name 4', count: 6, timestamp: 1739548594 },
		{ title: 'Title name 5', count: 9, timestamp: 1739548594 },
		{ title: 'Title name 6', count: 8, timestamp: 1739548594 },
		{ title: 'Title name 7', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 8', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 9', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 10', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 11', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 12', count: 7, timestamp: 1739548594 },
		{ title: 'Title name 13', count: 7, timestamp: 1739548594 }
	];

	let showSidebar = false;

	onMount(() => {
		showSidebar = true;
	});
</script>

<div class="rows">
	<div class="head">
		<h3 class="head-title">Better Counter</h3>
		<label class="show-sidebar-checkbox">
			<input type="checkbox" bind:checked={showSidebar} />
			visible
		</label>
	</div>
	{#each items as item}
		<Row
			on:click={() => handleClick(item.title)}
			title={item.title}
			count={item.count}
			timestamp={item.timestamp}
		></Row>
	{/each}
</div>
{#if showSidebar}
	<div class="sidebar" transition:fly={{ y: 200, duration: 200 }}>
		<h3 class="sidebar-title" style="bold" >{selectedTitle}</h3>
	</div>
{/if}

<style>
	.head {
		display: flex;
		width: 100%;
		height: 50px;
		background-color: var(--items-color);
		align-items: center;
		justify-content: space-between;
	}

	.head-title {
		margin-left: 20px;
	}

	.show-sidebar-checkbox {
		margin-right: 10px;
	}

	.rows {
		display: flex;
		flex-direction: column;
		width: 100%;
		gap: var(--items-gap);
		color: white;
	}

	.sidebar {
		background-color: #6c7d77;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 200px;
		animation: all 0.2s;
	}

	.sidebar-title {
		margin: 10px;
	}
</style>
