<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo.vue";
import axios from "axios";
export default {
  name: "Home",
  components: {
    Todos,
    AddTodo,
  },
  methods: {
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(() => {
          this.todos = this.todos.filter((todo) => todo.id !== id);
        })
        .catch((err) => console.warn(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed,
        })
        .then((response) => {
          this.todos = [...this.todos, response.data];
        })
        .catch((err) => console.warn(err));
    },
  },
  data() {
    return {
      todos: [],
    };
  },
  created() {
    axios("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((response) => {
        this.todos = response.data;
      })
      .catch((err) => {
        console.warn(err);
      });
  },
};
</script>

<style>
* {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}

.btn:hover {
  background: #666;
}
</style>
