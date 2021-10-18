<template>
  <div>
    <h2>List of tasks</h2>
    <router-link to="/">Home</router-link>
    <AddTodo 
      @add-todo="addTodo"
    />
    <TodoList 
      v-bind:todos="todos"
      @remove-todo="removeTodo"
    />
    <router-view/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import AddTodo from '@/components/AddTodo'
export default {
  name: 'App',
  components: {
    TodoList,
    AddTodo
  },
  data() {
    return {
      todos: [
        // {id: 1, title: "Купить хлеб", completed: false},
        // {id: 2, title: "Купить масло", completed: false},
        // {id: 3, title: "Купить молоко", completed: false},
      ]
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
      .then(response => response.json())
      .then(json => {
        this.todos = json
      })
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
