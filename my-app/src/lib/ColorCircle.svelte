<script lang="ts">
	export let color: string;
</script>

<div class="color-circle" style="background-color: {color};"></div>

<style>
	.color-circle {
		width: 35px;
		height: 35px;
		border-radius: 50%;
	}
</style>
