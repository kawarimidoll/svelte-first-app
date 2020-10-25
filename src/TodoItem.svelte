<script>
  import { createEventDispatcher } from "svelte";
  import { fly } from "svelte/transition";

  export let id;
  export let title;
  export let completed;

  const dispatch = createEventDispatcher();
  const deleteTodo = () => dispatch("deleteTodo", { id });
  const toggleComplete = () => dispatch("toggleComplete", { id });
</script>

<style>
  .todo-item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }
  .remove-item {
    cursor: pointer;
    margin-left: 15px;
  }
  .remove-item:hover {
    color: lightseagreen;
  }
  .todo-item-left {
    display: flex;
    align-items: center;
  }
  .todo-item-label {
    display: flex;
    cursor: pointer;
  }
  .todo-item-title {
    border: 1px solid white;
    margin-left: 12px;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>

<div class="todo-item">
  <div class="todo-item-left" transition:fly={{ y: 20, duration: 300 }}>
    <label class="todo-item-label">
      <input
        type="checkbox"
        bind:checked={completed}
        on:change={toggleComplete} />
      <div class="todo-item-title" class:completed>{title}</div>
    </label>
  </div>
  <div class="remove-item" on:click={deleteTodo}>x</div>
</div>
