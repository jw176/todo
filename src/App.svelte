<script>
	let items = [{"title": "Clean room", "desc" : ''}, {"title": "Code this app", "desc" : 'a description'}]

	let data = [];

	data.push({"name": "To Do", "items": items});
	data.push({"name": "Done", "items": items});

</script>

<main>
	<div class="main-container">
		{#each data as list, index}
			<div class="list">
				<h2>
					{list["name"]}
				</h2>

				{#each list["items"] as item, index}
				<div class="item">
					<h4>{item["title"]}</h4>
					<p>{item["desc"]}</p>
				</div>
			{/each}
			</div>
		{/each}
	</div>
</main>

<style>
	.item {
		background-color: #ffe5c4;
		padding: 4px;
		border-radius: 5px;
		text-align: left;
		margin: 5px;
	}

	.item:hover {
		margin: 0;
	}

	.item h4, p {
		margin: 0;
	}

	.item h4 {
		font-weight: 600;
	}

	.item p {
		font-size: 14px;
	}

	.list {
		/* background-color: #e7e7e7; */
		width: 200px;
		padding: 5px;
		text-align: center;
		margin: 10px auto;
		border-radius: 5px;
	}

	.list h2 {
		border-bottom: 1px solid #afafaf ;
		margin: 5px auto;
		background-color: #e7e7e7;
		border-radius: 4px;
	}

	.list h4 {
		text-align: left;
	}

	.main-container {
		display: flex;
		align-items: flex-start;
		justify-content: center;
		margin: auto;
		width: 90%;
		top: 40px;
		height: 80%;
		/* background-color: #afafaf; */
		border-radius: 5px;
	}

	main {
		text-align: center;
		padding: 1em;
		height: 100%;
		width: 100%;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>