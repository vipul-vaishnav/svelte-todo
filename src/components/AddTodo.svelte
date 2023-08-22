<script lang="ts">
  import type { ITodo } from '../types/ITodo'
  import { v4 as uuidv4 } from 'uuid'

  export let addTodo: (todo: ITodo) => void
  export let markUnmarkAllTodosCompleted: (e: MouseEvent) => void
  export let totalTodos: number
  export let completedTodos: number

  let todoText = ''

  const createNewTodoItem = (todoText: string): ITodo => {
    return { id: uuidv4(), completed: false, text: todoText }
  }

  const handleSubmit = () => {
    const todo = createNewTodoItem(todoText)
    addTodo(todo)
    todoText = ''
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <div class="flex w-full h-14">
    {#if totalTodos > 0}
      <input
        type="checkbox"
        name="toggle-all-todos"
        id="toggle-all-todos"
        class="hidden"
        on:change={markUnmarkAllTodosCompleted}
      />
      <label
        aria-label="Mark or Unmark all the todos as completed"
        for="toggle-all-todos"
        class="w-14 h-14 flex items-center justify-center cursor-pointer text-gray-300"
        class:text-gray-900={completedTodos === totalTodos}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1"
          stroke="currentColor"
          class="w-8 h-8"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>
      </label>
    {:else}
      <div class="w-14 h-14" />
    {/if}

    <!-- svelte-ignore a11y-autofocus -->
    <input
      bind:value={todoText}
      id="new-todo"
      class="flex-1 placeholder:italic focus:outline-none text-xl caret-gray-950 bg-transparent"
      placeholder="What needs to be done?"
      type="text"
      autofocus
    />
  </div>
</form>
