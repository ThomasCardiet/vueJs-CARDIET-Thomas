<template>
    <div class="main">
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key="todo" :class="{completed: todo.completed}">
          <div class="view">
            <input type="checkbox" v-model="todo.completed" class="toggle">
            <label>{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
        </li>
      </ul>
    </div>
</template>

<script>
export default {
  name: 'TodoList',
  props: ['filteredTodos'],
  methods: {
    deleteTodo(todo) {
      this.$emit('deleteTodo', todo);
    }
  }
}
</script>