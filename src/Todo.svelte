<script>
  export let todo = { id: "", text: "", completed: false };
  export let deleteTodo = () => {};
  export let updateTodo = () => {};
</script>

<div>
  <input
    type="checkbox"
    on:click={() => updateTodo({ ...todo, completed: !todo.completed })}
    checked={todo.completed} />
  <input
    type="text"
    on:change={event => updateTodo({ ...todo, text: event.target.value })}
    value={todo.text} />
  <a href="#" on:click|preventDefault={() => deleteTodo(todo)}>x</a>
</div>
