<template>
  <div>
    <h2>Todo application</h2>
    <router-link to="/">Home</router-link>
    <AddTodo @add-todo="addTodo"
    />
    <!-- <select v-model="filter">
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="not-completed">Not Completed</option>
    </select> -->
    <hr>
    <Loader v-if="loading"/>
    <TodoList
        v-else-if="todos.length"
        v-bind:todos="todos"
        @remove-todo="removeTodo"
    />
    <p v-else>No Todos!</p>
  </div>
</template>

<script>
import AddTodo from '@/components/AddTodo'
import TodoList from '@/components/TodoList'
import Loader from '@/components/Loader'

export default {
  name: 'App',
  data () {
    return {
      todos: [ ],
      loading: true,
      filter: 'all'
    }
  },
  components: {
    TodoList,
    AddTodo,
    Loader
  },
  mounted(){
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
    .then(response => response.json())
    .then(json => {
        setTimeout(() => {
            this.todos = json
            this.loading = false
         }, 1000)
        
    })
  },
    // computed: {
    //     filtered() { 
    //         if (this.filter === 'all') {
    //             return this.todos
    //         }
    //         if (this.filter === 'completed') {
    //             return this.todos.filter( t => t.completed )
    //         }
    //         if (this.filter === 'not-completed') {
    //             return this.todos.filter( t => !t.completed )
    //         }
           
    //     }
    // },
    methods: {
        removeTodo(id) {
        this.todos = this.todos.filter(t => t.id !== id)
        },
        addTodo(todo){
        this.todos.push(todo)
        }
    }
}

</script>