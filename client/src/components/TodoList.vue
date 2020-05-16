<template>
  <div>
    <p class="tasks">
      Completed Tasks:
      {{
        todos.filter(todo => {
          return todo.done === true;
        }).length
      }}
    </p>
    <p class="tasks">
      Pending Tasks:
      {{
        todos.filter(todo => {
          return todo.done === false;
        }).length
      }}
    </p>
    <todo v-bind:key="todo.id" v-on:delete-todo="deleteTodo" v-on:complete-todo="completeTodo" v-on:update-todo="updateTodo" v-for="todo in todos" :todo.sync="todo"></todo>
  </div>
</template>

<script type="text/javascript">
import sweetalert from "sweetalert";
import Todo from "./Todo";
import axios from "axios";
export default {
  props: ["todos"],
  components: {
    Todo
  },
  methods: {
    deleteTodo(todo) {
      sweetalert(
        {
          title: "Are you sure?",
          text: "This To-Do will be permanently deleted!",
          type: "warning",
          showCancelButton: true,
          confirmButtonColor: "#DD6B55",
          confirmButtonText: "Yes, delete it!",
          closeOnConfirm: false
        },
        () => {
          const todoIndex = this.todos.indexOf(todo);
          this.todos.splice(todoIndex, 1);
          const response = axios.delete(`http://127.0.0.1:3000/todos/${todo.id}`);
          sweetalert("Deleted!", "Your To-Do has been deleted.", "success");
        }
      );
    },
    updateTodo(todo) {
      const newtodo = {
        id: todo.id,
        desc: todo.project,
        isComplete: todo.done,
        title: todo.title
      };
      const response = axios.put(`http://127.0.0.1:3000/todos/${todo.id}`, newtodo);
      sweetalert("updated!", "Your To-Do has been update.", "success");
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
      todo.done = true;
      this.updateTodo(todo);
      sweetalert("Success!", "To-Do completed!", "success");
    }
  };
};
</script>

<style scoped>
p.tasks {
  text-align: center;
}
</style>
