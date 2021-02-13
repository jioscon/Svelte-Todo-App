<script>
  let now = new Date();
  let options = { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' };
  let today = now.toLocaleDateString('ko-KR', options)

  let inputTodo = '';
  let modifyTodo = '';
  let todoList = [
    { text: '스벨트로 Todo app 만들기', status: true, isEdit: false },
    { text: 'Reading an english book', status: false, isEdit: false },
    { text: 'Meeting some friends', status: false, isEdit: false },
    { text: 'Go to exercise and...', status: false, isEdit: false },
  ];

  function createTodo() {
    if (!inputTodo.trim()) {
      inputTodo = '';
      return;
    }
    todoList = [...todoList, { text: inputTodo, status: false, isEdit: false }];
    inputTodo = '';
  }

  function onEdit(index) {
    todoList[index].isEdit = true;
    modifyTodo = todoList[index].text;
  }

  function offEdit(index) {
    todoList[index].isEdit = false;
  }

  function updateTodo(index) {
    todoList[index].text = modifyTodo;
    todoList[index].isEdit = false;
  }

  function deleteTodo(index) {
    todoList.splice(index, 1);
    todoList = todoList;
  }

  $: sumItems = todoList.length;
  $: itemsLeft = todoList.filter((s) => !s.status).length;
</script>

<main class="container">
  <div class="todo-add">
    <input
      bind:value={inputTodo}
      on:keydown={(e) => {
        e.key === 'Enter' && createTodo();
      }}
      type="text"
      placeholder="new todo item.."
      aria-label="new todo item"
    />
    <button on:click={createTodo} type="button" aria-label="Add todo"
      >Add Todo</button
    >
  </div>

  <div class="todo-items-header">
    <h2>Todo List</h2>
    <span>{today}</span>
  </div>

  <div class="todo-items">
    {#each todoList as item, index}
      {#if item.isEdit}
        <div class="todo-item">
          <!-- svelte-ignore a11y-autofocus -->
          <input
            class="input-item"
            bind:value={modifyTodo}
            on:keydown={(e) => {
              e.key === 'Enter' && updateTodo(index);
            }}
            type="text" autofocus />
          <button
            on:click={() => offEdit(index)}
            type="button"
            aria-label="Cancel">Cancel</button
          >
        </div>
      {:else}
        <div class="todo-item">
          <label class:checked={item.status}>
            <input bind:checked={item.status} type="checkbox" />
            {item.text}
          </label>
          <div>
            <button
              on:click={() => onEdit(index)}
              type="button"
              aria-label="Edit">Edit</button
            >
            <span>·</span>
            <button
              on:click={() => deleteTodo(index)}
              type="button"
              aria-label="Del">Del</button
            >
          </div>
        </div>
      {/if}
    {:else}
      <div>Nothing...</div>
    {/each}
  </div>
  <div class="count">
    <span>Todo : {sumItems} </span>
    <span>{itemsLeft} {(itemsLeft > 1) ? 'items' : 'item'} left</span>
  </div>
</main>

<style>
  main {
    min-height: 100vh;
  }
</style>
