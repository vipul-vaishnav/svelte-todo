<script lang="ts">
  import type { ITodo } from '../types/ITodo'

  export let todo: ITodo
  export let toggleTodo: (e: MouseEvent, id: ITodo['id']) => void
  export let index: number
  export let removeTodo: (id: ITodo['id']) => void
  export let editTodo: (id: ITodo['id'], value: string) => void

  let editModeOn = false

  const enterEditMode = () => {
    editModeOn = true
  }

  function handleEdit(event: KeyboardEvent, id: string): void {
    let pressedKey = event.key
    let targetElement = event.target as HTMLInputElement
    let newTodo = targetElement.value

    switch (pressedKey) {
      case 'Escape':
        targetElement.blur()
        break
      case 'Enter':
        editTodo(id, newTodo)
        targetElement.blur()
        break
    }
  }

  function handleBlur(event: FocusEvent, id: string): void {
    let targetElement = event.target as HTMLInputElement
    let newTodo = targetElement.value

    editTodo(id, newTodo)
    targetElement.blur()
    editModeOn = false
  }
</script>

<li class="border-t border-gray-200 relative hover:bg-gray-100">
  <div class="group flex items-center w-full min-h-[46px]">
    <div>
      <input
        id={`todo-${index}`}
        name={`todo-${index}`}
        checked={todo.completed}
        class="hidden"
        type="checkbox"
        on:change={(e) => toggleTodo(e, todo.id)}
      />
      <label aria-label="Check todo" class="w-14 h-14 flex items-center justify-center" for={`todo-${index}`}>
        <div
          class="w-7 h-7 rounded-full border border-gray-300 hover:border-gray-500 cursor-pointer flex items-center justify-center"
          class:border-emerald-400={todo.completed}
          class:hover:border-emerald-400={todo.completed}
          class:text-emerald-400={todo.completed}
        >
          {#if todo.completed}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-4 h-4"
            >
              <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12.75l6 6 9-13.5" />
            </svg>
          {/if}
        </div>
      </label>
    </div>
    <span
      role="application"
      on:dblclick={enterEditMode}
      class="py-2 flex-1 truncate cursor-default font-medium"
      class:font-normal={todo.completed}
      class:line-through={todo.completed}
      class:text-gray-300={todo.completed}>{todo.text}</span
    >
    <div class="w-14 h-14 group-hover:hidden" />
    <button
      aria-label="Remove todo"
      on:click={() => removeTodo(todo.id)}
      class="hidden w-14 h-14 group-hover:flex items-center justify-center text-red-200"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="w-6 h-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
        />
      </svg>
    </button>
  </div>

  {#if editModeOn}
    <!-- svelte-ignore a11y-autofocus -->
    <input
      on:keydown={(event) => handleEdit(event, todo.id)}
      on:blur={(event) => handleBlur(event, todo.id)}
      class="absolute top-0 left-0 focus:outline-none w-full h-full px-14 border border-neutral-700"
      type="text"
      value={todo.text}
      autofocus
    />
  {/if}
</li>
