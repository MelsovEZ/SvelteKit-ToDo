<script lang="ts">
	// Define the type for a Todo item
	type Todo = {
		text: string;
		completed: boolean;
	};

	// State variables
	let todos: Todo[] = [];
	let newTodo: string = '';

	// Add a new todo item
	const addTodo = (): void => {
		if (newTodo.trim()) {
			todos = [...todos, { text: newTodo, completed: false }];
			newTodo = '';
		}
	};

	// Toggle the completion status
	const toggleTodo = (index: number): void => {
		todos = todos.map((todo, i) => (i === index ? { ...todo, completed: !todo.completed } : todo));
	};

	// Remove a todo item
	const removeTodo = (index: number): void => {
		todos = todos.filter((_, i) => i !== index);
	};
</script>

<div class="mx-auto mt-10 max-w-lg">
	<h1 class="mb-5 text-center text-3xl font-bold">Svelte To-Do App</h1>

	<div class="mb-4 flex items-center gap-2">
		<!-- Updated input to handle Enter key -->
		<input
			type="text"
			bind:value={newTodo}
			placeholder="Enter a new task..."
			class="w-full rounded-lg border px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
			on:keydown={(e) => e.key === 'Enter' && addTodo()}
		/>
		<button
			on:click={addTodo}
			class="rounded-lg bg-blue-500 px-4 py-2 text-white hover:bg-blue-600"
		>
			Add
		</button>
	</div>

	<ul class="space-y-2">
		{#each todos as todo, index}
			<li class="flex items-center justify-between rounded-lg bg-gray-100 p-3 shadow">
				<!-- Added keyboard toggle functionality -->
				<button class="flex items-center gap-3" on:click={() => toggleTodo(index)} tabindex="0">
					<input type="checkbox" checked={todo.completed} class="h-5 w-5" />
					<span class={`${todo.completed ? 'text-gray-500 line-through' : 'text-black'}`}>
						{todo.text}
					</span>
				</button>
				<button on:click={() => removeTodo(index)} class="text-red-500 hover:text-red-700">
					Remove
				</button>
			</li>
		{/each}
	</ul>
</div>
