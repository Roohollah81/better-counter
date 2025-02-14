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
		justify-content: space-between;
		gap:var(--fasele);
	}
	.btn {
		width: 10%;
		text-align: center;
		line-height: 3em;
		background-color: var(--ali);
		border: none;
	}
	.content {
		display: flex;
		align-items: center; 
		flex-direction: column;
		width: -webkit-fill-available;
		background-color: var(--ali);
	}
</style>
