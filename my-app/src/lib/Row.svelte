<script lang="ts">
	export let title: string;
	export let count: number;
	export let timestamp: number;
</script>

<div class="item">
	<button class="btn">-</button>
	<div class="content">
		<div class="title">{title}</div>
		<div class="title">{count}</div>
		<div class="title">{timestamp}</div>
	</div>
	<button class="btn">+</button>
</div>

<style>
	.item {
		display: flex;
		flex-direction: row;
		gap: var(--items-gap);
	}
	.btn {
		width: 10%;
		text-align: center;
		line-height: 5em;
		background-color: var(--items-color);
		border: none;
		font-size: 20px;
		font-weight: bold;
	}
	.content {
		display: flex;
		align-items: center;
		flex-direction: column;
		width: -webkit-fill-available;
		justify-content: space-evenly;
		background-color: var(--items-color);
	}
</style>
