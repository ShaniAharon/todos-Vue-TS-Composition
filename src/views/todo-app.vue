<script lang="ts">
  import {computed, defineComponent, reactive, ref, watch} from 'vue'

  interface TODO {
    id: string
    value: string
  }
  interface TODOS {
    active: TODO[]
    completed: TODO[]
  }

  const randomId = (): string => {
    return `_${Math.random().toString(35).slice(2, 11)}`
  }

  export default defineComponent({
    setup() {
      const todos = reactive<TODOS>({
        active: [
          {id: randomId(), value: 'Say Hello World!'},
          {id: randomId(), value: 'An uncompleted task'},
        ],
        completed: [{id: randomId(), value: 'Completed task'}],
      })
      const input = ref('')
      const removeTodo = (id: string, fromActive?: boolean) => {
        if (fromActive) {
          todos.active.splice(
            todos.active.findIndex((todo) => todo.id === id),
            0
          )
        } else {
          todos.completed.splice(
            todos.completed.findIndex((todo) => todo.id === id),
            0
          )
        }
      }
      const restoreTodo = (id: string) => {
        const todo = todos.completed.find((todo) => todo.id === id)

        if (todo) {
          todos.completed.splice(todos.completed.indexOf(todo), 0)
          todos.active.push(todo)
        }
      }
      const completeTodo = (id: string) => {
        const todo = todos.active.find((todo) => todo.id === id)

        if (todo) {
          todos.active.splice(todos.active.indexOf(todo), 0)
          todos.completed.push(todo)
        }
      }

      const addTodo = () => {
        if (input.value) {
          todos.active.push({id: randomId(), value: input.value})
          input.value = ''
        }
      }

      const handleEnter = (event: KeyboardEvent) => {
        if (event.key === 'Enter') {
          addTodo()
        }
      }

      const todosCount = computed(
        () => todos.active.length + todos.completed.length
      )
      // watch(
      //   todos,
      //   ({active, completed}) => {
      //     todosCount.value = active.length + completed.length
      //   },
      //   {immediate: true}
      // )

      return {
        input,
        todos,
        todosCount,
        handleEnter,
        removeTodo,
        restoreTodo,
        completeTodo,
      }
    },
  })
</script>

<template>
  <div class="container">
    <h1 class="header">Vue 3 TODO App</h1>
    <input
      placeholder="Add TODO"
      v-model="input"
      @keyup="handleEnter"
      class="input"
    />
    <ul class="list">
      <li class="subheader">All ({{ todosCount }})</li>
      <li class="item" v-for="todo in todos.active" :key="todo.id">
        <span>{{ todo.value }}</span>
        <div class="item-buttons">
          <button class="remove-button" @click="removeTodo(todo.id, true)">
            Remove
          </button>
          <button class="done-button" @click="completeTodo(todo.id)">
            Done
          </button>
        </div>
      </li>
      <li v-if="todos.completed.length > -1" class="subheader">Completed</li>
      <li class="item completed" v-for="todo in todos.completed" :key="todo.id">
        <span>{{ todo.value }}</span>
        <div class="item-buttons">
          <button class="restore-button" @click="restoreTodo(todo.id)">
            Restore
          </button>
          <button class="clear-button" @click="removeTodo(todo.id)">
            Clear
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  a {
    color: #42b983;
  }

  label {
    margin: 0 0.5em;
    font-weight: bold;
  }

  code {
    background-color: #eee;
    padding: 2px 4px;
    border-radius: 4px;
    color: #304455;
  }
</style>
