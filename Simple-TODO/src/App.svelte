<script>
	import {onMount} from 'svelte';
	
	var toDoList = [];
	var count = 0;
	
	var toDoItem = {
		itemNum: 0,
		itemDescription: ''
	};
	
	function formHandler(event) {
		event.preventDefault();

		if (toDoItem.itemDescription !== '') {
		
			toDoItem.itemNum = ++count;
		
			toDoList = [...toDoList, toDoItem];
			localStorage.setItem('data', JSON.stringify(toDoList));
			localStorage.setItem('count', count);
			toDoItem = {itemNum: 0, itemDescription: ''};
		}
	}

	function removeItem() {
		if (count > 0) {
			count = count - 1;
		
			toDoList.shift();
			for (let i = 0; i < toDoList.length; i++)
				toDoList[i].itemNum--;

			toDoList = toDoList;
			localStorage.setItem('count', count);
			localStorage.setItem('data', JSON.stringify(toDoList));
		}
	}

	onMount(() => {
		
		const savedList = localStorage.getItem('data');
		const savedCount = localStorage.getItem('count');

		if (savedList)
			toDoList = JSON.parse(savedList);

		if (savedCount)
			count = parseInt(savedCount);
	})
</script>

<main>
	<h1>TODO LIST:</h1>
	{#each toDoList as item}
		<ul>
			<li key={item.itemNum}>{item.itemNum}: {item.itemDescription}</li>
		</ul>
	{/each}
	<form on:submit={formHandler}>
		<label> 
			<input type="text" bind:value={toDoItem.itemDescription}>
		</label>
		<button type="submit">Submit</button>
	</form>
	<button on:click={removeItem}>Remove First Item</button>
</main>

<style>
	main {
		text-align: center;
	}

	ul {
		padding: 0;
		font-size: 30px;
		font-weight: bold;
	}

	h1 {
		font-size: 60px;
	}

	button, input {
		font-size: 20px;
		text-align: center;
	}
</style>