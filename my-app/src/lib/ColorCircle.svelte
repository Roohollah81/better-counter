<script lang="ts">
	export let color: string;
</script>

<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div
	class="color-circle"
	style="background-color: {color};"
></div>

<style>
	.color-circle {
		width: 35px;
		height: 35px;
		border-radius: 50%;
	}
	.color-circle:focus-visible {
		border: 1px solid rgb(255, 255, 255);
	}
</style>
‍