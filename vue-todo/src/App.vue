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
    return axios.get("http://127.0.0.1:3000/todos").then(response => {
      let todos = response.data.map(
        todo => ({
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
      this.todos.push(newTodo);
      sweetalert("Success!", "To-Do created!", "success");
    }
  }
};
</script>
