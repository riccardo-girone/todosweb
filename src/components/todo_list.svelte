<script>
	import { updated } from "$app/stores";
	import Icon from "./icon.svelte";
	import TodoItem from "./todo_item.svelte";
	import { onMount } from "svelte";

	let todos = [];
	let last_id = 0;

	const create_todo = async () => {
		let todo = {
			id: ++last_id,
			task: "",
			done: false,
			priority: 1,
		};
		console.log("CREATE: ", todo);

		localStorage.setItem("todo" + todo.id, JSON.stringify(todo));

		todos = [...todos, todo];
	};

	const change_todo_item = async (e) => {
		switch (e.detail_type) {
			case "update":
				update_item(e.detail.id);
				break;
			case "delete":
				delete_item(e.detail.id);
				break;
		}
	};

	const delete_item = (id) => {
		console.log("DELETE:", id);
		todos = todos.filter((t) => t.id != id);
	};

	onMount(async () => {
		for (let i = 0; i < localStorage.length; i++) {
			const key = localStorage.key(i);
            const keyn = + key.substring(4)
            if(keyn >= last_id) {
                last_id = keyn
            }
			const todo = JSON.parse(localStorage.getItem(key));
			if (todo != null) {
				todos.push(todo);
			}
		}

		todos = [...todos];
	});
</script>

<h1 class="app-title">ToDos</h1>

<div class="todo-list">
	<div class="header"><Icon name="tag" /></div>
	<div class="header"><Icon name="task_alt" /></div>
	<div class="header"><Icon name="list" /></div>
	<div class="header"><Icon name="schedule" /></div>
	<div class="header header-last">
		<Icon name="add_box" handler={create_todo} />
	</div>

	{#each todos as todo}
		<TodoItem {todo} on:change={change_todo_item} />
	{/each}
</div>

<style>
	@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap");

	.todo-list {
		display: grid;
		grid-template-columns: 1fr 1fr 4fr 2fr 1fr;
		border: 0px solid blue;
		width: 95%;
		margin: auto;
	}

	.header {
		border-bottom: 1px solid #e7ecee;
		border-right: 1px solid #e7ecee;
		text-align: center;
		padding-bottom: 20px;
	}

	.header-last {
		border-right: none;
	}

	.app-title {
		font-family: "Montserrat", serif;
		margin-top: 0px;
		font-size: 60px;
		opacity: 0.3px;
	}
</style>
