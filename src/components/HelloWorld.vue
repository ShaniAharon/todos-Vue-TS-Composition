<script setup lang="ts">
  import {ref} from 'vue'

  defineProps<{msg: string}>()
  //ref - in other words, ref() allows us to create a "reference" to any value and pass it around without losing reactivity. This capability is quite important as it is frequently used when extracting logic into Composable Functions.
  const count = ref(0)
</script>

<template>
  <h1>{{ msg }}</h1>
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
