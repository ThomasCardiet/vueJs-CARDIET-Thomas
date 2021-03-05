<template>
  <section class="todoapp">
    <header class="header">

      <div id="error-block" v-show="error">
        [!] Erreur: {{ error }}
      </div>

      <div id="informations">
        <p>{{ date }}</p>
        <p id="title">VueJs Tutorial ToDo List</p>
        <p>{{ current_tasks }}</p>
      </div>
      <label v-show="hasTodo">
        <input type="checkbox" id="check-all" v-model="allDone">
        Cocher toutes les tâches
      </label>
      <NewTodo v-on:addTodo="addTodo($event)"></NewTodo>
    </header>
    <TodoList v-on:deleteTodo="deleteTodo($event)" :filteredTodos="filteredTodos"></TodoList>
    <footer class="footer" v-show="hasTodo">
      <span class="todo-count">{{ not_done_tasks }}</span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
      </ul>
      <button class="clear-completed" v-show="checked" @click.prevent="deleteChecked">Supprimer les tâches terminées</button>
      <br>
      <button id="clear-all" @click.prevent="deleteAll">Supprimer toutes les tâches</button>
    </footer>
    <br>
  </section>
</template>

<script>
import NewTodo from './NewTodo'
import TodoList from './TodoList'

export default {
  name: 'TodoCard',
  components: {
    NewTodo,
    TodoList,
  },
  data () {
    return {
      days: ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"],
      months: ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"],

      todos: [],
      filter: 'all',
      error: ''
    }
  },
  methods: {
    addTodo(value) {
      if(value.length <= 0) return this.error = "Tâche trop petite"
      this.error = '';
      this.todos.push({
        completed: false,
        name: value
      })
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter(i => i !== todo);
    },
    deleteChecked() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    deleteAll() {
      this.todos = [];
    }
  },
  computed: {
    date: function () {
      let today_date = new Date();
      return this.days[today_date.getDay()] + " " + today_date.getDate() + " " + this.months[today_date.getMonth()];
    },

    current_tasks: function () {
      return this.todos.length + " " + (this.todos.length > 1 ? "tâches" : "tâche");
    },
    not_done_tasks: function () {
      return this.remaining + " " + (this.remaining > 1 ? "tâches" : "tâche") + " à faire";
    },
    allDone: {
      get () {
        return this.remaining === 0;
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value;
        })
      }
    },
    remaining () { return this.todos.filter(todo => !todo.completed).length },
    checked () { return this.todos.filter(todo => todo.completed).length },
    hasTodo() {
      return this.todos.length > 0;
    },
    filteredTodos () {
      switch (this.filter){
        case 'todo':
          return this.todos.filter(todo => !todo.completed);
        case 'done':
          return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    }
  }
}
</script>