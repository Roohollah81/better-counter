<script lang="ts">
	import Row from '$lib/Row.svelte';
	import ColorCircle from '$lib/ColorCircle.svelte';
	import { fly } from 'svelte/transition';
	import ContributionTimeline from '$lib/ContributionTimeline.svelte';
	let selectedTitle = '';
	let showSidebar = false;
	let showTimeLine = false;
	let showBarchart = false;
	let showAddItem = false;
	let goalCount = 0;
	let newItemName = '';
	let newItemLifeTime = 'Lifetime';
	let showEmptyFieldWarningMessage = false;
	let selectedColor: string = 'Gray';

	const options = [
		{ value: 'Hourly', label: 'Hourly' },
		{ value: 'Daily', label: 'Daily' },
		{ value: 'Weekly', label: 'Weekly' },
		{ value: 'Monthly', label: 'Monthly' },
		{ value: 'Yearly', label: 'Yearly' },
		{ value: 'Lifetime', label: 'Lifetime' }
	];

	function handleColorSelection(color: string) {
		selectedColor = color;
	}

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

	let selectedItem: {};
	function handleClickOnRowContent(title: string, item: any) {
		selectedTitle = title;
		showSidebar = true;
		selectedItem = item;
	}

	const circles = [
		{ color: 'Gray' },
		{ color: 'Blue' },
		{ color: 'Purple' },
		{ color: 'Brown' },
		{ color: 'Indigo' },
		{ color: 'Orange' },
		{ color: 'Pink' },
		{ color: 'Green' }
	];

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
				Chart
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
			<button on:click={() => removeItem(index)}>Remove Item</button>
		{/each}
	</div>
	<button
		class="add-content"
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
				<button class="button-49" role="button">Time Line</button>
				<button class="button-49" role="button">Bar Chart</button>
			</div>
			<h3 class="sidebar-title" style="bold">{selectedTitle}</h3>
			<ContributionTimeline {selectedItem} />
			{#if showBarchart}{/if}
			{#if showTimeLine}{/if}
		</div>
	{/if}
	{#if showAddItem}
		<div class="menu" transition:fly={{ y: 200, duration: 200 }}>
			<h3 class="menu-title" style="bold">Add counter</h3>
			<input
				class="select-item-name"
				type="text"
				bind:value={newItemName}
				placeholder={showEmptyFieldWarningMessage ? 'Give the counter a name' : 'Counter name'}
			/>
			<div class="life-time-and-goal">
				<select class="select-life-time" bind:value={newItemLifeTime}>
					{#each options as option}
						<option value={option.value}>{option.label} </option>
					{/each}
				</select>
				<!-- svelte-ignore a11y_click_events_have_key_events -->
				<!-- svelte-ignore a11y_no_static_element_interactions -->
				<div class="select-goal">
					<span
						class="material-symbols-outlined"
						on:click={() => {
							decreaseGoal();
						}}
					>
						remove
					</span>
					<div class="goal-count">
						{goalCount}
					</div>
					<span
						class="material-symbols-outlined"
						on:click={() => {
							increaseGoal();
						}}
					>
						add
					</span>
				</div>
			</div>
			<div class="select-color">
				{#each circles as circle}
					<ColorCircle color={circle.color} on:click={() => handleColorSelection(circle.color)} />
				{/each}
			</div>
			<!-- svelte-ignore a11y_click_events_have_key_events -->
			<!-- svelte-ignore a11y_no_static_element_interactions -->
			<div class="save-cancel">
				<div
					class="cancel"
					on:click={() => {
						closeAddNewItemWindow();
					}}
				>
					Cancel
				</div>
				<div
					class="save"
					on:click={() => {
						addNewItem();
					}}
				>
					Save
				</div>
			</div>
		</div>
	{/if}
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
	.add-content {
		position: fixed;
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
		display: flex;
		flex-direction: column;
		align-content: center;
		flex-wrap: wrap;
		justify-content: space-between;
	}
	.menu-title {
		margin: 15px 0px 0px 10px;
	}
	.select-item-name {
		width: 304px;
		height: 50px;
		left: 14px;
		background-color: #414141;
		border: 1px solid white;
		border-radius: 5px;
		padding-left: 15px;
		outline: none;
		font-size: 20px;
		color: white;
	}
	.select-life-time {
		width: 190px;
		height: 50px;
		background-color: #414141;
		border: 1px solid white;
		border-radius: 5px;
		padding-left: 15px;
		outline: none;
		font-size: 17px;
		color: white;
	}
	.select-goal {
		width: 120px;
		height: 48px;
		border: 1px solid white;
		border-radius: 5px;
		right: 0;
		display: flex;
		justify-content: space-around;
		align-items: center;
		font-size: 20px;
	}
	.life-time-and-goal {
		display: flex;
		justify-content: space-between;
	}
	.select-color {
		width: 321px;
		height: 50px;
		border-radius: 5px;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.save-cancel {
		width: 321px;
		height: 50px;
		border-radius: 5px;
		display: flex;
		justify-content: flex-end;
		margin-bottom: 10px;
	}
	.cancel {
		width: 80px;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.save {
		width: 80px;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.body {
		width: 100%;
		height: 100%;
		background-color: #307872;
		position: absolute;
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
