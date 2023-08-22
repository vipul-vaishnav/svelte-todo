<script lang="ts">
  import type { ITodo } from './../types/ITodo'
  import type { Filter } from './../types/Filters'

  import AddTodo from './AddTodo.svelte'
  import TodoItem from './TodoItem.svelte'

  // state
  let todos: Array<ITodo> = [
    { id: '1e4a59703af84', text: 'Todo 1', completed: true },
    { id: '9e09bcd7b9349', text: 'Todo 2', completed: false },
    { id: '9e4273a51a37c', text: 'Todo 3', completed: false },
    { id: '53ae48bf605cc', text: 'Todo 4', completed: false }
  ]
  let filter: Filter = 'all'

  // reactive values
  $: totalTodos = todos.length
  $: pendingTodos = todos.filter((item) => !item.completed).length
  $: completedTodos = totalTodos - pendingTodos
  $: filteredTodos = filterTodos(todos, filter)

  // functions || handlers
  const markUnmarkAllTodosCompleted = (e: MouseEvent) => {
    if (completedTodos === totalTodos) {
      todos = todos.map((item) => ({ ...item, completed: false }))
    } else {
      todos = todos.map((item) => ({ ...item, completed: true }))
    }
  }

  const addTodo = (todoItem: ITodo) => {
    todos = [todoItem, ...todos]
  }

  const removeTodo = (id: ITodo['id']) => {
    todos = todos.filter((todo) => todo.id !== id)
  }

  const editTodo = (todoId: ITodo['id'], todoValue: string) => {
    todos = todos.map((item) => (item.id === todoId ? { ...item, text: todoValue } : item))
  }

  const toggleTodo = (e: MouseEvent, id: ITodo['id']) => {
    const checked = (e.target as HTMLInputElement).checked
    todos = todos.map((todo) => (todo.id === id ? { ...todo, completed: checked } : todo))
  }

  const setFilter = (type: Filter) => {
    filter = type
  }

  const filterTodos = (todos: ITodo[], type: Filter) => {
    switch (type) {
      case 'all':
        return todos
      case 'active':
        return todos.filter((item) => !item.completed)
      case 'completed':
        return todos.filter((item) => item.completed)
    }
  }

  const clearCompleted = () => {
    todos = todos.filter((todo) => !todo.completed)
  }
</script>

<div class="bg-gray-200 min-h-screen p-6">
  <h1 class="text-center text-8xl text-red-400">todos</h1>

  <div class="my-4 max-w-screen-sm mx-auto bg-neutral-50 text-neutral-900 rounded shadow-md border border-gray-200">
    <AddTodo {completedTodos} {addTodo} {markUnmarkAllTodosCompleted} {totalTodos} />

    <ul>
      {#each filteredTodos as todo, index (todo.id)}
        <TodoItem {index} {todo} {toggleTodo} {removeTodo} {editTodo} />
      {/each}
    </ul>

    {#if totalTodos > 0}
      <div class="flex justify-between items-center p-3 text-neutral-500 text-sm border border-gray-200">
        <span class="todo-count">{pendingTodos} {pendingTodos === 1 ? 'item' : 'items'} left</span>
        <div class="flex items-center gap-3">
          <button
            on:click={() => setFilter('all')}
            class="border border-opacity-50 rounded px-2 py-1"
            class:border-2={filter === 'all'}
            class:border-red-200={filter === 'all'}
            class:hover:border-red-200={filter === 'all'}>All</button
          >
          <button
            on:click={() => setFilter('active')}
            class="border border-opacity-50 rounded px-2 py-1"
            class:border-2={filter === 'active'}
            class:border-red-200={filter === 'active'}
            class:hover:border-red-200={filter === 'active'}>Active</button
          >
          <button
            on:click={() => setFilter('completed')}
            class="border border-opacity-50 rounded px-2 py-1"
            class:border-2={filter === 'completed'}
            class:border-red-200={filter === 'completed'}
            class:hover:border-red-200={filter === 'completed'}>Completed</button
          >
        </div>
        {#if completedTodos > 0}
          <button on:click={clearCompleted} class="hover:underline">Clear completed</button>
        {:else}
          <button on:click={() => {}} class="invisible">Clear completed</button>
        {/if}
      </div>
    {/if}
  </div>
</div>
