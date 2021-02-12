<script>
  let now = new Date();
  const months = [
    'January',
    'Fabruary',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December',
  ];
  const week = ['Sun', 'Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat'];
  let year = now.getFullYear();
  let month = months[now.getMonth()];
  let date = now.getDate();
  let day = week[now.getDay()];
  let today = `${month} ${date} ${day}, ${year}`;

  let newItem = '';
  let todoList = [
    { text: '스벨트로 Todo app 만들기', status: true },
    { text: 'Reading an english book', status: false },
    { text: 'Meeting some friends', status: false },
    { text: 'Go to exercise and...', status: false },
  ];

  function addToList() {
    if (!newItem.trim()) {
      newItem = '';
      return;
    }
    todoList = [...todoList, { text: newItem, status: false }];
    newItem = '';
  }

  function deleteToList(index) {
    todoList.splice(index, 1);
    todoList = todoList;
  }

  $: sumItems = todoList.length;
  $: leftItems = todoList.filter((s) => !s.status).length;
</script>

<main class="container">
  <div class="todo-add">
    <input
      bind:value={newItem}
      on:keydown={(e) => {
        e.key === 'Enter' && addToList();
      }}
      type="text"
      placeholder="new todo item.."
      aria-label="new todo item"
    />
    <button on:click={addToList} type="button" aria-label="Add todo"
      >Add Todo</button
    >
  </div>

  <div class="todo-items-header">
    <h2>Todo List</h2>
    <span>{today}</span>
  </div>

  <div class="todo-items">
    {#each todoList as item, index}
      <div class="todo-item">
        <label class:checked={item.status}>
          <input bind:checked={item.status} type="checkbox" />
          {item.text}
        </label>
        <button
          on:click={() => deleteToList(index)}
          type="button"
          aria-label="Del">X</button
        >
      </div>
      {:else}
      <div>Nothing...</div>
    {/each}
  </div>
  <div class="count">
    <span>Todo : {sumItems} </span>
    <span>remaining : {leftItems}</span>
  </div>
</main>

<style>
  main {
	min-height: 100vh;
}
</style>
