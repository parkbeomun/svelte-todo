<script lang="ts">
	import { Icon, Styles, Form, FormGroup, FormText, Input, Label } from 'sveltestrap';
	const ENTER_KEY = 13;
	const ESCAPE_KEY = 27;

	let currentFilter = 'all'
	let items = [];
	let editing = null;


	function createNew(event) {
		if(event.which == ENTER_KEY) {
			items = items.concat({
				id: uuid(),
				description: event.target.value,
				completed: false
			})

			event.target.value = '';
			console.log("items > "+items)
		}
	}

	function handleEdit() {

	}

	function submit() {

	}

	function remove(index) {
		items = items.slice(0, index).concat(items.slice(index + 1));
	}

	function uuid() {
		return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
			var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
			return v.toString(16);
		});
	}

	$: filtered = currentFilter === 'all'
		? items
		: currentFilter === 'completed'
			? items.filter(item => item.completed)
			: items.filter(item => !item.completed)
</script>

<main>
	<h1>Todos</h1>
		<Input
			on:keydown={createNew}/>
		{#if items.length > 0}
			<ul class="todo-list">
				{#each filtered as item, index (item.id)}
					<li class="{item.completed ? 'completed' : ''} {editing === index ? 'editing' : ''}">
						<div class="view">
							<input class="toggle" type="checkbox" bind:checked={item.completed}>
							<label on:dblclick="{() => editing = index}">{item.description}</label>
							<button on:click="{() => remove(index)}">X</button>
						</div>

						{#if editing === index}
							<input
									value='{item.description}'
									id="edit"
									class="edit"
									on:keydown={handleEdit}
									on:blur={submit}
									autofocus
							>
						{/if}
					</li>
				{/each}
			</ul>
		{/if}
</main>