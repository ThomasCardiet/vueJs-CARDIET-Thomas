<template>
  <TodoCard></TodoCard>
</template>

<script>

import TodoCard from './components/TodoCard'

export default {
  name: 'app',
  components: {
    TodoCard
  },
  data () {
    return {
      msg: 'Hello, et bienvenue sur la demo TodoList en Vue.js!'
    }
  }
}

</script>

<style src="./components/todo.css"></style>