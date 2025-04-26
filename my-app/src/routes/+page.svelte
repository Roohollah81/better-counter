<script lang="ts">
	import Menu from '../lib/Menu.svelte';
	import EditPanel from '$lib/EditPanel.svelte';
	import Row from '$lib/Row.svelte';
	import ContributionTimeline from '$lib/ContributionTimeline.svelte';
	import BarChart from '$lib/BarChart.svelte';
	import { fly } from 'svelte/transition';
	let selectedTitle = '';
	let showSidebar = false;
	let showTimeLine = false;
	let showBarchart = false;
	let showAddItem = false;
	let showEditPanel = false;
	let goalCount = 0;
	let newItemName = '';
	let newItemLifeTime = 'Lifetime';
	let showEmptyFieldWarningMessage = false;
	let selectedColor: string = 'Gray';

	let items: any[] = [];
	if (typeof window !== 'undefined') {
		const savedItems = localStorage.getItem('items');
		if (savedItems) {
			items = JSON.parse(savedItems);
			items.forEach((item) => {
				item.latestContributionTimestamp = item.contributions.reduce((latest: string, c: any) => {
					return c.timestamp > latest ? c.timestamp : latest;
				}, '');
			});
		} else {
			items = [
				// {
				// 	title: 'Title 1',
				// 	contributions: [],
				// 	latestContributionTimestamp: '', // Initialize as empty
				// 	color: 'blue'
				// },
				// {
				// 	title: 'Title 2',
				// 	contributions: [],
				// 	latestContributionTimestamp: '', // Initialize as empty
				// 	color: 'green'
				// }
			];
		}
	}

	$: {
		if (typeof window !== 'undefined') {
			localStorage.setItem('items', JSON.stringify(items));
		}
	}

	if (typeof window !== 'undefined') {
		const savedItems = localStorage.getItem('items');
		if (savedItems) {
			items = JSON.parse(savedItems);
		}
	}

	let selectedItem: any;
	function handleClickOnRowContent(title: string, item: any) {
		selectedTitle = title;
		showSidebar = true;
		showTimeLine = false;
		showBarchart = false;
		selectedItem = item;
	}

	$: data = selectedItem ? selectedItem.contributions : [];

	$: transformedData = selectedItem
		? selectedItem.contributions.map((contribution: { date: any; count: any }) => ({
				date: contribution.date,
				activities: Array(contribution.count).fill({})
			}))
		: [];

	function addNewItem() {
		if (!newItemName) {
			showEmptyFieldWarningMessage = true;
		} else {
			showAddItem = false;
			let new_item = {
				title: newItemName,
				color: selectedColor,
				contributions: [{ date: new Date().toISOString(), count: goalCount }]
			};
			items = [...items, new_item];
		}
	}

	function closeAddNewItemWindow() {
		showAddItem = false;
	}

	function increaseGoal() {
		goalCount++;
	}

	function decreaseGoal() {
		if (goalCount - 1 >= 0) {
			goalCount--;
		}
	}

	// Function to handle updates from Row
	function handleUpdate(itemIndex: number) {
		items[itemIndex].contributions = [...items[itemIndex].contributions];
		items[itemIndex].latestContributionTimestamp = items[itemIndex].contributions.reduce(
			(latest: string, c: any) => {
				return c.timestamp > latest ? c.timestamp : latest;
			},
			''
		);
		items = items; // Trigger reactivity
	}

	// Function to remove an item
	function removeItem(itemIndex: number) {
		items.splice(itemIndex, 1); // Remove the item
		items = items; // Trigger reactivity
	}

	function updateItem() {
    // Find the index of the selected item
    const index = items.findIndex(item => item.title === selectedItem.title);
    
    if (index !== -1) {
      // Update the item with new values
      items[index] = {
        ...items[index],
        title: selectedItem.title, // Updated from the edit panel
        color: selectedColor       // Updated from the edit panel
      };
      
      // Trigger reactivity
      items = items;
      
      // Close the edit panel
      showEditPanel = false;
    }
  }

  function deleteItem() {
    // Find the index of the selected item
    const index = items.findIndex(item => item.title === selectedItem.title);
    
    if (index !== -1) {
      // Remove the item
      items.splice(index, 1);
      
      // Trigger reactivity
      items = items;
      
      // Close the edit panel and sidebar
      showEditPanel = false;
      showSidebar = false;
    }
  }
</script>

<link
	rel="stylesheet"
	href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0"
/>

<div class="body">
	<div class="rows">
		<div class="head">
			<h3 class="head-title">Better Counter</h3>
			<label class="show-sidebar-checkbox">
				<input type="checkbox" bind:checked={showSidebar} />
				Panel
			</label>
		</div>
		{#each items as item, index}
			<Row
				on:click={() => handleClickOnRowContent(item.title, item)}
				on:update={() => handleUpdate(index)}
				title={item.title}
				contributions={item.contributions}
				latestContributionTimestamp={item.latestContributionTimestamp}
				selectedColor={item.color}
			></Row>
		{/each}
	</div>
	<button
		class="add-edite-content"
		on:click={() => {
			showEmptyFieldWarningMessage = false;
			showAddItem = true;
			goalCount = 0;
			newItemName = '';
			newItemLifeTime = 'Lifetime';
			selectedColor = 'Gray';
		}}
	>
		<span class="material-symbols-outlined"> add </span>
	</button>
	{#if showSidebar}
		<div class="sidebar" transition:fly={{ y: 200, duration: 200 }}>
			<!-- svelte-ignore a11y_no_redundant_roles -->
			<div class="select-category">
				<button
					class="button-49"
					role="button"
					on:click={() => {
						showTimeLine = true;
						showBarchart = false;
					}}>Time Line</button
				>
				<button
					class="button-49"
					role="button"
					on:click={() => {
						showBarchart = true;
						showTimeLine = false;
					}}>Bar Chart</button
				>
			</div>
			<h3 class="sidebar-title" style="bold">{selectedTitle}</h3>
			{#if showBarchart}
				<BarChart {data} />
			{/if}
			{#if showTimeLine}
				<ContributionTimeline {transformedData} />
			{/if}
			<button
				class="add-edite-content"
				on:click={() => {
					showEmptyFieldWarningMessage = false;
					showAddItem = true;
					goalCount = 0;
					newItemName = '';
					newItemLifeTime = 'Lifetime';
					selectedColor = 'Gray';
				}}
			>
				<span class="material-symbols-outlined"> edit </span>
			</button>
		</div>
	{/if}
	<Menu
		bind:showAddItem
		bind:showEmptyFieldWarningMessage
		bind:goalCount
		bind:newItemName
		bind:newItemLifeTime
		bind:selectedColor
		on:close={closeAddNewItemWindow}
		on:save={addNewItem}
		on:increaseGoal={increaseGoal}
		on:decreaseGoal={decreaseGoal}
	/>
	<EditPanel
		bind:showEditPanel
		bind:selectedItem
		bind:selectedColor
		on:save={updateItem}
		on:delete={deleteItem}
	/>
</div>
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
		top: 50px;
		left: 0;
		right: 0;
		bottom: 0;
		color: white;
		position: absolute;
		animation: all 0.2s;
		background-color: #3d3466;
		overflow-x: auto; /* Enable horizontal scrolling if needed */
	}
	.sidebar-title {
		margin-left: 10px;
		padding-top: 10px;
	}
	.add-edite-content {
		position: absolute;
		bottom: 20px;
		right: 20px;
		padding: 20px 20px;
		background-color: #e7e7e7;
		color: #1e1e1e;
		border: none;
		border-radius: 20px;
		cursor: pointer;
		font-size: 0px;
	}
	.body {
		width: 375px;
		height: 667px;
		margin: 20px auto;
		border: 1px solid #ccc;
		overflow: auto;
		position: relative;
		box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
		background-color: #307872;
		overflow-x: hidden; /* Prevent horizontal overflow */
	}
	.select-category {
		display: flex;
		justify-content: space-around;
		cursor: pointer;
		top: 10px;
		position: relative;
		border-bottom: 1px solid;
		padding-bottom: 10px;
	}
	/* CSS */
	.button-49,
	.button-49:after {
		width: 150px;
		height: 50px;
		line-height: 50px;
		font-size: 15px;
		font-family: 'Bebas Neue', sans-serif;
		background: linear-gradient(45deg, transparent 5%, #ff013c 5%);
		border: 0;
		color: #fff;
		letter-spacing: 1px;
		box-shadow: 6px 0px 0px #00e6f6;
		outline: transparent;
		position: relative;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
	}

	.button-49:after {
		--slice-0: inset(50% 50% 50% 50%);
		--slice-1: inset(80% -6px 0 0);
		--slice-2: inset(50% -6px 30% 0);
		--slice-3: inset(10% -6px 85% 0);
		--slice-4: inset(40% -6px 43% 0);
		--slice-5: inset(80% -6px 5% 0);

		content: 'ALTERNATE TEXT';
		display: block;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: linear-gradient(45deg, transparent 3%, #00e6f6 3%, #00e6f6 5%, #ff013c 5%);
		text-shadow:
			-3px -3px 0px #f8f005,
			3px 3px 0px #00e6f6;
		clip-path: var(--slice-0);
	}

	.button-49:hover:after {
		animation: 1s glitch;
		animation-timing-function: steps(2, end);
	}

	@keyframes glitch {
		0% {
			clip-path: var(--slice-1);
			transform: translate(-20px, -10px);
		}
		10% {
			clip-path: var(--slice-3);
			transform: translate(10px, 10px);
		}
		20% {
			clip-path: var(--slice-1);
			transform: translate(-10px, 10px);
		}
		30% {
			clip-path: var(--slice-3);
			transform: translate(0px, 5px);
		}
		40% {
			clip-path: var(--slice-2);
			transform: translate(-5px, 0px);
		}
		50% {
			clip-path: var(--slice-3);
			transform: translate(5px, 0px);
		}
		60% {
			clip-path: var(--slice-4);
			transform: translate(5px, 10px);
		}
		70% {
			clip-path: var(--slice-2);
			transform: translate(-10px, 10px);
		}
		80% {
			clip-path: var(--slice-5);
			transform: translate(20px, -10px);
		}
		90% {
			clip-path: var(--slice-1);
			transform: translate(-10px, 0px);
		}
		100% {
			clip-path: var(--slice-1);
			transform: translate(0);
		}
	}

	@media (min-width: 768px) {
		.button-49,
		.button-49:after {
			width: 200px;
			height: 86px;
			line-height: 88px;
		}
	}
</style>
