<script>
	import { createEventDispatcher } from "svelte";
	import Cell from "./cell.svelte";
	import Icon from "./icon.svelte";
	import Priority from "./priority.svelte";

	export let todo;
	let old_priority = todo.priority;
	const dispatch = createEventDispatcher();

	const toggle_status = () => {
		todo.done = !todo.done;
	};

	const edit_task = () => {
		document.getElementById(todo.id).blur();
	};

	$: {
		if (todo.priority != old_priority) {
			old_priority = todo.priority;
			item_change("update");
		}
	}

	const item_change = (type) => {
		dispatch("change", { type: type, id: todo.id });
	};
</script>

<Cell>
	{todo.id}
</Cell>
<Cell>
	{#if todo.done == false}
		<Icon name="circle" handler={toggle_status} color="red" />
	{:else}
		<Icon name="task_alt" handler={toggle_status} />
	{/if}
</Cell>
<Cell>
	<input
		type="text"
		class="todo-item-input-text {todo.done == true ? 'text-done' : ''}"
		id={todo.id}
		placeholder="inserisci il nuovo ToDo"
		bind:value={todo.task}
		on:change={edit_task}
	/>
</Cell>
<Cell>
	<Priority blind:prio={todo.priority} disabled={todo.done} />
</Cell>
<Cell last>
	<Icon
		name="delete_forever"
		color="red"
		handler={() => item_change("delete")}
	/>
</Cell>

<style>
	.todo-item-input-text {
		border: none;
		width: 90%;
		height: 30px;
		font-size: 20px;
		color: #525252;
	}

	.todo-item-input-text:focus {
		background-color: rgb(204, 229, 253);
		color: black;
		padding: 4px;
		padding-left: 10px;
	}

	.text-done {
		text-decoration: line-through;
		opacity: 0.3;
	}
</style>
