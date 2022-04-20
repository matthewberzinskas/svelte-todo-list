<script>
  import Todo from "./Todo.svelte";
  import FilterButton from "./FilterButton.svelte";
  import MoreActions from "./MoreActions.svelte";
  import NewTodo from "./NewTodo.svelte";

  export let todos = [];

  $: totalTodos = todos.length;
  $: completedTodos = todos.filter((todo) => todo.completed).length;

  let newTodoName = "";
  let newTodoId;
  $: {
    if (totalTodos === 0) {
      newTodoId = 1;
    } else {
      newTodoId = Math.max(...todos.map((t) => t.id)) + 1;
    }
  }

  function addTodo(name) {
    todos = [...todos, { id: newTodoId, name, completed: false }];
  }

  function removeTodo(todo) {
    todos = todos.filter((t) => t.id !== todo.id);
  }

  function updateTodo(todo) {
    const i = todos.findIndex((t) => t.id === todo.id);
    todos[i] = { ...todos[i], ...todo };
  }

  let filter = "all";
  const filterTodos = (filter, todos) =>
    filter === "active"
      ? todos.filter((t) => !t.completed)
      : filter === "completed"
      ? todos.filter((t) => t.completed)
      : todos;

  const checkAllTodos = (completed) => {
    todos = todos.map((t) => {
      // shorter version: todos = todos.map(t => ({...t, completed}))
      return { ...t, completed: completed };
    });
  };

  const removeCompletedTodos = () =>
    (todos = todos.filter((t) => !t.completed));
</script>

<!-- Todos.svelte -->
<div class="todoapp stack-large">
  <!-- NewTodo -->
  <NewTodo autofocus on:addTodo={(e) => addTodo(e.detail)} />

  <!-- Filter -->
  <FilterButton bind:filter />

  <!-- TodosStatus -->
  <h2 id="list-heading">
    {completedTodos} out of {totalTodos} items completed
  </h2>

  <!-- To-dos -->
  <ul class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter, todos) as todo (todo.id)}
      <li class="todo">
        <Todo
          {todo}
          on:remove={(e) => removeTodo(e.detail)}
          on:update={(e) => updateTodo(e.detail)}
        />
      </li>
    {:else}
      <li>Nothing to do here!</li>
    {/each}
  </ul>

  <hr />

  <!-- MoreActions -->
  <MoreActions
    {todos}
    on:checkAll={(e) => checkAllTodos(e.detail)}
    on:removeCompleted={removeCompletedTodos}
  />
</div>
