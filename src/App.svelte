<script>
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';

	let items = [{"id": 1, "title": "Clean room", "desc" : '', "done": false}, {"id": 2, "title": "Code this app", "desc" : 'a description', "done": false}];

	let id = 3;

	const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});


	function addItem(input) {
		if (input.value) {
		console.log('working sort of');
		
		let newItem = {id: id++, title: input.value, desc: '', done: false}
		// items.push(newItem);

		items = [...items, newItem];
		input.value = '';
		}
	}

	function remove(todo) {
		items = items.filter(t => t !== todo);
	}

	function markAsDone(todo, done) {
		todo.done = done;
		remove(todo);
		items = items.concat(todo);
	}

</script>

<main>
	<div class="input">
		<h2>
			Enter an item
		</h2>
		<input type="text" on:keydown={e => e.key === 'Enter' && addItem(e.target)}>
	</div>

	<div class="main-container">
			<div class="list">
				<h2>
					Todo
				</h2>

				{#each items.filter((obj) => !obj["done"]) as item, index (item["id"])}
					<div class="item" on:click={markAsDone(item, true)} in:receive="{{key: item.id}}"
					out:send="{{key: item.id}}">
						<h4>{item["title"]}</h4>
						<p>{item["desc"]}</p>
					</div>
				{/each}
			</div>
			<div class="list">
				<h2>
					Done
				</h2>

				{#each items.filter((obj) => obj["done"]) as item, index (item["id"])}
					<div class="completed item" on:click={markAsDone(item, false)} 	in:receive="{{key: item.id}}"
					out:send="{{key: item.id}}">
						<h4>{item["title"]}</h4>
						<p>{item["desc"]}</p>
					</div>
				{/each}
			</div>
	</div>
</main>

<style>
	.completed {
		background-color: greenyellow !important;
	}

	.input h2 {
		margin: 5px;
	}

	.input input {
		width: 60%;
		max-width: 550px;
	}

	/* .input input {
		margin-bottom: 40px;
	} */

	.item {
		background-color: #ffe5c4;
		padding: 4px;
		border-radius: 5px;
		text-align: left;
		margin: 5px;
		cursor: pointer;
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
		margin: 10px 40px;
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