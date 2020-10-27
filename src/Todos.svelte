<script>
  import TodoItem from "./TodoItem.svelte";
  let todos = [
    { id: 1, title: "First item", completed: false },
    { id: 2, title: "Second item", completed: false },
    { id: 3, title: "Third item", completed: false },
  ];
  let newTodoTitle = "";
  let currentFilter = "all";
  let nextId = 4;
  const addTodo = (event) => {
    if (event.key !== "Enter") {
      return;
    }
    todos = [...todos, { id: nextId, title: newTodoTitle, completed: false }];
    nextId += 1;
    newTodoTitle = "";
  };
  $: filteredTodos =
    currentFilter === "all"
      ? todos
      : currentFilter === "completed"
      ? todos.filter((t) => t.completed)
      : todos.filter((t) => !t.completed);
  const handleDeleteTodo = (event) => {
    todos = todos.filter((t) => t.id !== event.detail.id);
  };
  const handleToggleComplete = (event) => {
    const idx = todos.findIndex((t) => t.id === event.detail.id);
    todos = [
      ...todos.slice(0, idx),
      { ...todos[idx], completed: !todos[idx].completed },
      ...todos.slice(idx + 1),
    ];
  };
  $: todosRemaining = todos.filter((t) => !t.completed).length;
  const checkAllTodos = (event) => {
    todos = todos.map((t) => ({ ...t, completed: event.target.checked }));
  };
  const updateFilter = (newFilter) => {
    currentFilter = newFilter;
  };
  const clearCompleted = () => {
    todos = todos.filter((t) => !t.completed);
  };
</script>

<style>
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .todo-input {
    width: 100%;
    padding: 10px, 20px;
    font-size: 18px;
    margin-bottom: 20px;
  }
  .inner-container {
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }
  button {
    @apply appearance-none;
    background-color: white;
    font-size: 14px;
  }
  button:hover {
    @apply bg-lightseagreen;
  }
  button:focus {
    @apply outline-none;
  }
  .active {
    @apply bg-lightseagreen;
  }
</style>

<div class="container">
  <h1>Svelte Todo App</h1>
  <input
    type="text"
    class="todo-input"
    placeholder="Insert todo item..."
    bind:value={newTodoTitle}
    on:keydown={addTodo} />

  {#each filteredTodos as todo}
    <div class="todo-item">
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete} />
    </div>
  {/each}

  <div class="inner-container flex items-center justify-between">
    <div>
      <label>
        <input
          type="checkbox"
          class="mr-4"
          on:change={checkAllTodos} />Check All
      </label>
    </div>
    <div>{todosRemaining} items left</div>
  </div>

  <div class="inner-container">
    <div>
      <button
        on:click={() => updateFilter('all')}
        class:active={currentFilter === 'all'}>All</button>
      <button
        on:click={() => updateFilter('active')}
        class:active={currentFilter === 'active'}>Active</button>
      <button
        on:click={() => updateFilter('completed')}
        class:active={currentFilter === 'completed'}>Completed</button>
    </div>
    <div><button on:click={clearCompleted}>Clear Completed</button></div>
  </div>
</div>
