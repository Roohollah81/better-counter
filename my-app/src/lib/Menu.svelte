<!-- src/lib/Menu.svelte -->
<script lang="ts">
	import ColorCircle from '$lib/ColorCircle.svelte';
	import { createEventDispatcher } from 'svelte';
	import { fly } from 'svelte/transition';

	export let showAddItem: boolean;
	export let showEmptyFieldWarningMessage: boolean;
	export let goalCount: number;
	export let newItemName: string;
	export let newItemLifeTime: string;
	export let selectedColor: string;

	export const options = [
		{ value: 'Hourly', label: 'Hourly' },
		{ value: 'Daily', label: 'Daily' },
		{ value: 'Weekly', label: 'Weekly' },
		{ value: 'Monthly', label: 'Monthly' },
		{ value: 'Yearly', label: 'Yearly' },
		{ value: 'Lifetime', label: 'Lifetime' }
	];

	export const circles = [
		{ color: 'Gray' },
		{ color: 'Blue' },
		{ color: 'Purple' },
		{ color: 'Brown' },
		{ color: 'Indigo' },
		{ color: 'Orange' },
		{ color: 'Pink' },
		{ color: 'Green' }
	];

	// Define dispatched events
	const dispatch = createEventDispatcher();

	function handleColorSelection(color: string) {
		selectedColor = color;
	}
</script>

{#if showAddItem}
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<!-- svelte-ignore a11y_click_events_have_key_events -->
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
			<div class="select-goal">
				<span class="material-symbols-outlined" on:click={() => dispatch('decreaseGoal')}>
					remove
				</span>
				<div class="goal-count">
					{goalCount}
				</div>
				<span class="material-symbols-outlined" on:click={() => dispatch('increaseGoal')}>
					add
				</span>
			</div>
		</div>
		<div class="select-color">
			{#each circles as circle}
				<ColorCircle color={circle.color} on:click={() => handleColorSelection(circle.color)} />
			{/each}
		</div>
		<div class="save-cancel-section">
			<div class="save-cancel-button" on:click={() => dispatch('close')}>Cancel</div>
			<div class="save-cancel-button" on:click={() => dispatch('save')}>Save</div>
		</div>
	</div>
{/if}

<style>
	.menu {
		animation: all 0.2s;
		background-color: #2f3c3d;
		width: 350px;
		height: 300px;
		position: fixed;
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
		z-index: 1000;
	}
	.menu-title {
		margin: 15px 0px 0px 10px;
	}
	.select-item-name {
		width: 304px;
		height: 50px;
		background-color: #2f3c3d;
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
		background-color: #2f3c3d;
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
		cursor: pointer;
	}
	.save-cancel-section {
		width: 321px;
		height: 50px;
		border-radius: 5px;
		display: flex;
		justify-content: flex-end;
		margin-bottom: 10px;
	}
	.save-cancel-button {
		width: 80px;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		border: 2px solid white;
		border-radius: 5px;
	}
</style>
