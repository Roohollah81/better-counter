<!-- src/routes/+page.svelte -->
<script>
	import { onMount } from 'svelte';
	import Row from '../lib/Row.svelte';
	import { fly } from 'svelte/transition';

	const items = [
		{ title: 'asdfsf', count: 7, timestamp: 7924156416546 },
		{ title: 'sdfgds', count: 7, timestamp: 7924156416546 },
		{ title: 'gdsfg', count: 7, timestamp: 7924156416546 },
		{ title: 'xcvzv', count: 7, timestamp: 7924156416546 },
		{ title: 'sdgvbnfjkjk', count: 7, timestamp: 7924156416546 },
		{ title: 'oyuio', count: 7, timestamp: 7924156416546 },
		{ title: 'hkghl', count: 7, timestamp: 7924156416546 },
		{ title: 'hkghl', count: 7, timestamp: 7924156416546 },
		{ title: 'hkghl', count: 7, timestamp: 7924156416546 },
		{ title: 'hkghl', count: 7, timestamp: 7924156416546 },
		{ title: 'uio', count: 7, timestamp: 7924156416546 },
		{ title: 'uio', count: 7, timestamp: 7924156416546 },
		{ title: 'uio', count: 7, timestamp: 7924156416546 }
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
		<Row title={item.title} count={item.count} timestamp={item.timestamp}></Row>
	{/each}
</div>
{#if showSidebar}
	<div class="sidebar" transition:fly={{ y: 200, duration: 200 }}>salam</div>
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
		background-color: aquamarine;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 200px;
		animation: all 0.2s;
	}
</style>
