<template>
  <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
</template>

<script>
  export default {
    name: 'NewTodo',
    data () {
      return {
        newTodo: '',
      }
    },
    methods: {
      addTodo() {
        this.$emit('addTodo', this.newTodo);
        this.newTodo = "";
      }
    }
  }
</script>
