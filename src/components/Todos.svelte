<script>
  import Card, {
    Content,
    PrimaryAction,
    Actions,
    ActionButtons,
    ActionIcons,
  } from '@smui/card';
  import IconButton, { Icon } from '@smui/icon-button';
  import LayoutGrid, { Cell } from '@smui/layout-grid';
  import Button, { Label } from '@smui/button';
  import FormField from '@smui/form-field';
  import Checkbox from '@smui/checkbox';
  import Textfield from '@smui/textfield';
  import TodoItem from './TodoItem.svelte';

  let newTodoTitle = '';
  let currentFilter = 'all';
  let nextId = 4;

  let todos = [
    {
      id: 1,
      title: 'My first todo',
      completed: false,
    },
    {
      id: 2,
      title: 'My second todo',
      completed: false,
    },
    {
      id: 3,
      title: 'My third todo',
      completed: false,
    },
  ];

  function addTodo(event) {
    if (event.key === 'Enter') {
      todos = [
        ...todos,
        {
          id: nextId,
          completed: false,
          title: newTodoTitle,
        },
      ];

      nextId = nextId + 1;
      newTodoTitle = '';
    }
  }

  $: todosRemaining = filteredTodos.filter((todo) => !todo.completed).length;
  $: filteredTodos =
    currentFilter === 'all'
      ? todos
      : currentFilter === 'completed'
      ? todos.filter((todo) => todo.completed)
      : todos.filter((todo) => !todo.completed);

  function checkAllTodos(event) {
    todos.forEach((todo) => (todo.completed = event.target.checked));
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter((todo) => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter((todo) => todo.id !== event.detail.id);
  }

  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex((todo) => todo.id === event.detail.id);
    const updatedTodo = {
      ...todos[todoIndex],
      completed: !todos[todoIndex].completed,
    };

    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex + 1),
    ];
  }
</script>

<div class="container">
  <LayoutGrid>
    <Cell span={4} class="">
      <div class="logo-container">
        <img class="logo" src={'/img/logo_svelte.png'} alt="Svelte Logo" />
      </div>
    </Cell>
    <Cell span={8}>
      <Card>
        <Content>
          <h2>Svelte Todo App</h2>

          <div class="todo-input">
            <Textfield
              color="primary"
              variant="outlined"
              label="Insert todo item ..."
              style="width: 100%;"
              bind:value={newTodoTitle}
              on:keydown={addTodo}
            />
          </div>

          {#each filteredTodos as todo}
            <TodoItem
              {...todo}
              on:deleteTodo={handleDeleteTodo}
              on:toggleComplete={handleToggleComplete}
            />
          {/each}

          <div class="inner-container">
            <FormField>
              <Checkbox on:change={checkAllTodos} />
              <span slot="label">Check All</span>
            </FormField>
            <div>{todosRemaining} items left</div>
          </div>
        </Content>
        <Actions>
          <ActionButtons>
            <Button
              on:click={() => updateFilter('all')}
              color="primary"
              variant={currentFilter === 'all' ? 'raised' : 'outlined'}
            >
              <Label>All</Label>
            </Button>
            <Button
              on:click={() => updateFilter('active')}
              color="primary"
              variant={currentFilter === 'active' ? 'raised' : 'outlined'}
            >
              <Label>Active</Label>
            </Button>
            <Button
              on:click={() => updateFilter('completed')}
              color="primary"
              variant={currentFilter === 'completed' ? 'raised' : 'outlined'}
            >
              <Label>Completed</Label>
            </Button>
          </ActionButtons>
          <ActionIcons>
            <Button
              on:click={clearCompleted}
              color="primary"
              variant="outlined"
            >
              <Label>Clear Completed</Label>
            </Button>
          </ActionIcons>
        </Actions>
      </Card>
    </Cell>
  </LayoutGrid>
</div>

<style>
  .container {
    max-width: 800px;
    margin: 10px auto;
  }
  .logo-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }
  .logo {
    display: block;
    margin: 20px auto;
    width: 200px;
  }
  .inner-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 15px;
    margin-bottom: 13px;
  }
  .todo-input {
    margin-bottom: 1rem;
  }
</style>
