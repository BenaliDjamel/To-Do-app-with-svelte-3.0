<script>
  import { fade } from "svelte/transition";

  let todo = "";
  let selectedTodo = {};
  let todos = [
    {
      id: 1,
      name: "learn laravel",
      complete: false
    },
    {
      id: 2,
      name: "learn vue",
      complete: false
    },
    {
      id: 3,
      name: "learn svelte",
      complete: false
    }
  ];

  function addTodo() {
    let obj = {
      id: todos.length + 1,
      name: todo
    };

    todos = [...todos, obj];
    todo = "";
  }

  function deleteTodo(id) {
    todos = todos.filter(item => item.id != id);
  }

  function selectTodo(id, index) {
    let todoItem = todos.find(item => item.id === id);
    todo = todoItem ? todoItem.name : "";
    selectedTodo.id = todoItem ? todoItem.id : "";
    selectedTodo.index = index;
  }

  function editTodo() {
    todos.splice(selectedTodo.index, 1, {
      id: selectedTodo.id,
      name: todo,
      complete: false
    });
    todo = "";
    todos = todos;
  }
  $: isAllTodoChecked = todos.some(item => item.complete);
  $: isTodoItemSelected = selectedTodo.id ? true : false;

  function checkAll() {
    if (!isAllTodoChecked) {
      todos.forEach(item => (item.complete = true));
    } else {
      todos.forEach(item => (item.complete = false));
    }
    todos = todos;
  }

  function deleteMultipl() {
    todos = todos.filter(item => !item.complete);
  }
</script>

<style>
  li {
    margin-bottom: 10px;
    list-style-type: none;
  }
  .del {
    margin-left: 20px;
    color: red;
    font-weight: bold;
    cursor: pointer;
  }

  .completed {
	  text-decoration: line-through
  }
</style>

<h1>My ToDo</h1>

<div>
  <label for="todo">Enter your Task:</label>

  <input type="text" id="todo" bind:value={todo} />

  <button on:click={addTodo} disabled={todo.trim().length === 0}>Add</button>

  <button
    on:click={editTodo}
    disabled={todos.length === 0 || !isTodoItemSelected || todo.trim().length === 0}>
    edit
  </button>

  <button on:click={checkAll} disabled={todos.length === 0}>
    {#if !isAllTodoChecked}check all{:else}Unckeck all{/if}
  </button>

  <button
    on:click={deleteMultipl}
    disabled={!isAllTodoChecked || todos.length === 0}>
    delete Multiple
  </button>

  <ul>

    {#each todos as { id, name, complete }, i}
      <li transition:fade>
        <input type="checkbox" bind:checked={complete} value={name} />
        <span on:click={() => selectTodo(id, i)}  class={complete ? 'completed': ''}>#{id} - {name}</span>
        <span on:click={() => deleteTodo(id)} class="del">X</span>
      </li>
    {/each}

  </ul>
</div>
