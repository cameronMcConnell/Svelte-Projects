<script>
	import {onMount} from 'svelte';
	
	var itemList = [];
	var input = '';

	function handleAddingItem(event) {
		
		event.preventDefault();
		
		if (input !== '') {
			
			var newItem = {
				completed: false,
				id: new Date().toString().slice(0,24),
				description: input
			};

			itemList = [...itemList, newItem];
			input = '';
			
			localStorage.setItem('list', JSON.stringify(itemList));
		}
	}

	function handleRemove(id, completed) {
			if (completed) {
				itemList = itemList.filter(item => item.id !== id);
				localStorage.setItem('list', JSON.stringify(itemList));
			}
	}

	onMount(() => {
		const savedList = localStorage.getItem('list');
		
		if (savedList)
			itemList = JSON.parse(savedList);
	})

</script>

<main>
	<h1>TODO List:</h1>
	{#each itemList as item}
	<ul>
		<li key={item.id}>
			<input type="checkbox" bind:checked={item.completed} class="check">
			<span class="date">{item.id};</span><br> <span class="desc">{item.description}.</span><br>
			<button on:click={handleRemove(item.id, item.completed)} class="remove">Remove</button>
		</li>
	</ul>
	{/each}
	<form on:submit={handleAddingItem}>
		<label>
			<input bind:value={input} type="text">
		</label>
		<button type="submit">Add</button>
	</form>
</main>

<style>
	main {text-align: center;}
	
	h1 {font-size: 65px;}

	ul {
		padding: 0;
		font-size: 25px;
	}

	button, input {
		font-size: 20px;
		text-align: center;
	}

	.check {
		width: 20px;
		height: 20px;
	}

	.remove {
		margin-top: 10px;
	}

	.date {
		font-weight: bold;
	}

	.desc {
		font-size: 25px;
	}

</style>