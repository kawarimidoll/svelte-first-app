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
  .animation-duration-03 {
    animation-duration: 0.3s;
  }
  .completed {
    @apply line-through;
    color: gray;
  }
</style>

<div class="mb-4 flex items-center justify-between animation-duration-03">
  <div class="flex items-center" transition:fly={{ y: 20, duration: 300 }}>
    <label class="flex cursor-pointer">
      <input
        type="checkbox"
        bind:checked={completed}
        on:change={toggleComplete} />
      <div class="ml-4" class:completed>{title}</div>
    </label>
  </div>
  <div
    class="cursor-pointer ml-8 hover:text-lightseagreen"
    on:click={deleteTodo}>
    x
  </div>
</div>
