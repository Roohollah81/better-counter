<div class="container">
	<slot></slot>
</div>

<style>
	:global(body) {
		margin: 0;
		--items-gap: 8px;
		font-family: Roboto;
		--items-color: rgb(105, 105, 105);
	}
	.container {
		background-color: #3a3a3a;
	}
</style>
