<!-- src/routes/+page.svelte -->
<script>
	// You can add JavaScript logic here if needed
</script>

<div class="container">
	<table>
		<thead>
			<tr>
				<th class="header" colspan="3">BetterCounter</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td class="reduce">-</td>
				<td></td>
				<td class="increase">+</td>
			</tr>
			<tr>
				<td class="reduce">-</td>
				<td></td>
				<td class="increase">+</td>
			</tr>
			<tr>
				<td class="reduce">-</td>
				<td></td>
				<td class="increase">+</td>
			</tr>
			<tr>
				<td class="reduce">-</td>
				<td></td>
				<td class="increase">+</td>
			</tr>
			<tr>
				<td class="reduce">-</td>
				<td></td>
				<td class="increase">+</td>
			</tr>
		</tbody>
	</table>
</div>

<style>
	body {
		margin: 0;
		padding: 0;
		font-family: Arial, sans-serif;
		background-color: #f4f4f4;
	}

	.container {
		padding: 20px;
		max-width: 300px;
		height: 600px;
		margin: auto;
		text-align: center;
		border: 1px solid black;
		background-color: #333;
	}

	.header {
		text-align: left;
	}

	.reduce {
		font-size: 20px;
	}

	.increase {
		font-size: 20px;
	}

	h1 {
		/* color: #333; */
	}

	p {
		font-size: 16px;
		/* color: #666; */
	}

	table {
		border-collapse: collapse;
		width: 100%;
		color: rgb(180, 178, 174);
	}
	th,
	td {
		border: 1px solid black;
		padding: 8px; /* Optional: adds space inside cells */
		text-align: center; /* Optional: aligns text to the left */
	}
</style>
