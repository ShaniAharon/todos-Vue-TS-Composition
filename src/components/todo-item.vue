<template>
  <li class="item" :class="{dark: darkMode}">
    <span>{{ todo.value }}</span>
    <div class="item-buttons">
      <button class="remove-button" @click="removeTodo">Remove</button>
      <button class="done-button" @click="completeTodo">Done</button>
    </div>
  </li>
</template>

<script lang="ts">
  import {defineComponent, inject, PropType, Ref} from 'vue'

  interface TODO {
    id: string
    value: string
  }

  export default defineComponent({
    props: {
      todo: {
        required: true,
        type: Object as PropType<TODO>,
      },
      removeTodo: {
        required: true,
        type: Object as PropType<() => void>,
      },
      completeTodo: {
        required: true,
        type: Object as PropType<() => void>,
      },
    },
    setup() {
      const darkMode = inject<Ref<boolean>>('dark-mode')

      return {
        darkMode,
      }
    },
  })
</script>

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
