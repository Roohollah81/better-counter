<script lang="ts">
	export let color: string;
	export let active: boolean = false; // Add this line
</script>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="color-circle {active ? 'active' : ''}" style="background-color: {color};" on:click></div>

<style>
	.color-circle {
		width: 35px;
		height: 35px;
		border-radius: 50%;
	}
</style>
