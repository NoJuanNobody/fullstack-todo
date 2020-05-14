<template>
  <div id="app">
    <h1 class="ui dividing centered header">Vue.js Todo App</h1>
    <div class="ui three column centered grid">
      <div class="column">
        <todo-list v-bind:todos="todos"></todo-list>
        <create-todo v-on:create-todo="createTodo"></create-todo>
      </div>
    </div>
  </div>
</template>

<script>
import sweetalert from "sweetalert";
import TodoList from "./components/TodoList";
import CreateTodo from "./components/CreateTodo";
import axios from "axios";

export default {
  name: "app",
  components: {
    TodoList,
    CreateTodo
  },
  data() {
    return {
      todos: null
    };
  },
  mounted() {
    axios.get("http://127.0.0.1:3000/todos").then(response => {
      let todos = response.data.map(
        todo => ({
          id: todo.id,
          title: todo.title,
          project: todo.desc,
          done: todo.isComplete
        }),
        []
      );
      this.todos = todos;
    });
  },
  methods: {
    createTodo(newTodo) {
      const newTodoAdapter = {
        title: newTodo.title,
        desc: newTodo.project,
        isComplete: newTodo.done,
      }
      
      const response = axios.post('http://127.0.0.1:3000/todos', newTodoAdapter).then(response => {
        newTodo.id = response.data.id
        this.todos.push(newTodo);
      }
      )
      sweetalert("Success!", "To-Do created!", "success");
    }
  }
};
</script>
