<template>
  <div id="app">
    <todo-header></todo-header>
    <todo-input @addTodo="addTodo"></todo-input>
    <todo-list :tododata="todos" @removeTodo="removeTodo"
    @toggleTodo="toggleTodo"></todo-list>
    <todo-footer @clearAll="clearAll"></todo-footer>
    
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  name: 'App',
  data: () => {
    return {
      todos: [],
    }
  },
  methods:{
    addTodo(item) {
        const todoObj = {
          item: item,
          completed: false,
        }
        localStorage.setItem(item, JSON.stringify(todoObj))
        this.todos.push(todoObj)
    },
    removeTodo(todoItem, idx){
      localStorage.removeItem(todoItem)
      this.todos.splice(idx,1)
    },
    toggleTodo(idx) {
      this.todos[idx].completed = !this.todos[idx].completed
      localStorage.removeItem(this.todos[idx].item)
      localStorage.setItem(this.todos[idx].item, JSON.stringify(this.todos[idx]))
    },
    clearAll() {
      this.todos = []
      localStorage.clear()
    },
  },
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  created() {
    if(localStorage.length > 0) {
      for(var i = 0; i < localStorage.length; i++){
        const todoItem = localStorage.getItem(localStorage.key(i)) 
        this.todos.push(JSON.parse(todoItem))
      }
    }
  },
}
</script>

<style>
body {
  background-color: beige;
  text-align: center;
}
</style>
