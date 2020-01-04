<script>
  import Todo from "./Todo.svelte";
  import debounce from "./util/debounce";
  import uuid from "uuid";

  let todos = JSON.parse(window.localStorage.getItem("todos")) || [];
  let savedTodos = todos;
  let text = "";

  const trySaveTodos = debounce(
    () => {
      if (window.localStorage) {
        window.localStorage.setItem("todos", JSON.stringify(todos));
      }
    },
    100,
    true
  );

  $: if (todos !== savedTodos) {
    trySaveTodos();
  }

  const addTodo = () => {
    if (text.trim() === "") return;
    todos = [...todos, { id: uuid.v4(), text, completed: false }];
    text = "";
  };

  const deleteTodo = todo => {
    todos = todos.filter(currentTodo => currentTodo.id !== todo.id);
  };

  const updateTodo = todo => {
    todos = todos.map(currentTodo => {
      if (todo.id === currentTodo.id) {
        return todo;
      }

      return currentTodo;
    });
  };
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Todo List</h1>
  <form action="#" on:submit|preventDefault={addTodo}>
    <input type="text" bind:value={text} />
  </form>
  <form action="#" on:input={trySaveTodos} on:change={trySaveTodos}>
    <section class="todos">
      {#each todos as todo}
        <Todo {todo} {deleteTodo} {updateTodo} />
      {/each}
    </section>
  </form>
</main>
