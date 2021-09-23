<div class="todo-item">
  <div class="todo-item-left" transition:fly={{ y: 20, duration: 300 }}>
    <FormField>
      <Checkbox bind:checked={completed} on:change={toggleComplete} />
      <span slot="label" class="todo-item-label" class:completed>
        {title}
      </span>
    </FormField>
  </div>
  <IconButton class="material-icons" style="margin-bottom: 0" on:click={deleteTodo}>close</IconButton>
</div>

<script>
  import { createEventDispatcher } from 'svelte';
  import { fly } from 'svelte/transition';
  import IconButton from '@smui/icon-button';
  import FormField from '@smui/form-field';
  import Checkbox from '@smui/checkbox';

  export let id;
  export let title;
  export let completed;

  const dispatch = createEventDispatcher();

  function deleteTodo() {
    dispatch('deleteTodo', {
      id: id,
    });
  }

  function toggleComplete() {
    dispatch('toggleComplete', {
      id: id,
    });
  }
</script>

<style>
  .todo-item {
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
  }

  .todo-item-left {
    display: flex;
    align-items: center;
  }

  .todo-item-label {
    border: 1px solid white;
    margin-left: 12px;
  }

  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>
