<template>
  <div>
    <h2>List of tasks</h2>
    <router-link to="/">Home</router-link>
    <hr>
    <AddTodo 
      @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not completed">Not completed</option>
    </select>
    <Loader v-if="loading" />
    <TodoList 
      v-else-if="filterredTodos.length"
      v-bind:todos="filterredTodos"
      @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
    <router-view/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
import Loader from '@/components/Loader'
export default {
  name: 'App',
  components: {
    TodoList,
    AddTodo,
    Loader
  },
  data() {
    return {
      todos: [
        // {id: 1, title: "Купить хлеб", completed: false},
        // {id: 2, title: "Купить масло", completed: false},
        // {id: 3, title: "Купить молоко", completed: false},
      ],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
      .then(response => response.json())
      .then(json => {
        this.todos = json
        this.loading = false
      })
  },
  // Следит за моделями (за изменениями)
  // watch: {
  //   filter(value){
      
  //   }
  // },
  computed: {
    filterredTodos() {
      if(this.filter === 'all') {
        return this.todos
      }
      if(this.filter === 'completed') {
        return this.todos.filter(t => t.completed)
      }
      if(this.filter === 'not completed') {
        return this.todos.filter(t => !t.completed)
      }
    }
  },
  methods: {
    removeTodo(id){
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo) {
      this.todos.push(todo)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
