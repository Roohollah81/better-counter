<script lang="ts">
	import Row from '../lib/Row.svelte';
	import { fly } from 'svelte/transition';
	let selectedTitle = '';
	let showSidebar = false;
	let showAddItem = false;

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

	const itemColors = ['blue', 'pink', 'yellow', 'gray', 'green'];

	function addNewItem() {
		showAddItem = true;
		let itemName;
		let intervalToDisplay;
		let goal;
		let itemColor;

		let new_item = {
			title: 'itemName',
			count: 0,
			timestamp: new Date()
		};
		items.push(new_item);
	}
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
<button
	class="add-content"
	on:click={() => {
		addNewItem();
	}}
>
	<span class="material-symbols-outlined"> add </span>
</button>
{#if showSidebar}
	<div class="sidebar" transition:fly={{ y: 200, duration: 200 }}>
		<h3 class="sidebar-title" style="bold">{selectedTitle}</h3>
	</div>
{/if}
{#if showAddItem}
	<div class="menu" transition:fly={{ y: 200, duration: 200 }}>
		<h3 class="menu-title" style="bold">Add counter</h3>
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
		cursor: pointer;
	}
	.rows {
		width: 100%;
		color: white;
		display: flex;
		cursor: pointer;
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
	.add-content {
		position: fixed;
		bottom: 20px;
		right: 20px;
		padding: 20px 20px;
		background-color: #e7e7e7;
		color: #414141;
		border: none;
		border-radius: 20px;
		cursor: pointer;
		font-size: 0px;
	}
	.menu {
		animation: all 0.2s;
		background-color: #414141;
		width: 350px;
		height: 300px;
		position: absolute;
		color: rgb(226, 221, 221);
		top: 50%;
		left: 50%;
		border-radius: 10px;
		transform: translate(-50%, -50%);
	}
	.menu-title {
		margin: 25px;
	}
</style>
