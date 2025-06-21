<!-- src/lib/EditPanel.svelte -->
<script lang="ts">
	import ColorCircle from '$lib/ColorCircle.svelte';
	import { fly } from 'svelte/transition';
	import { createEventDispatcher } from 'svelte';
	
	export let showEditPanel: boolean;
	export let selectedItem: any;
	export let selectedColor: string;

	const dispatch = createEventDispatcher();

	const circles = [
		{ color: 'Gray' },
		{ color: 'Blue' },
		{ color: 'Purple' }
		// ... other colors
	];
</script>

{#if showEditPanel}
	<div class="edit-panel" transition:fly={{ y: 200, duration: 200 }}>
		<h3 class="panel-title">Edit Counter</h3>

		<input
			class="edit-item-name"
			type="text"
			bind:value={selectedItem.title}
			placeholder="Counter name"
		/>

		<div class="color-selection">
			{#each circles as circle}
				<ColorCircle
					color={circle.color}
					on:click={() => (selectedColor = circle.color)}
				/>
			{/each}
		</div>

		<div class="action-buttons">
			<button class="delete-btn" on:click={() => dispatch('delete')}> Delete Counter </button>
			<button class="save-btn" on:click={() => dispatch('save')}> Save Changes </button>
		</div>
	</div>
{/if}

<style>
	.edit-panel {
		/* Similar styles to your menu */
		z-index: 1000;
		background-color: #414141;
		width: 350px;
		height: 300px;
		position: absolute;
		color: rgb(226, 221, 221);
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		/* ... other styles ... */
	}

	.action-buttons {
		display: flex;
		justify-content: space-between;
	}

	.delete-btn {
		background-color: #ff3b30;
		/* ... button styles ... */
	}
</style>
