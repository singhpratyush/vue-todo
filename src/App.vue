<template>
  <div id="app">
    <AddTodoItem v-on:todo-add="handleTodoAdd" />
    <TodoItem
      v-for="todoId in todo_ids"
      :key="todoId"
      :id="todoId"
      :isCompleted="todos[todoId].isCompleted"
      :task="todos[todoId].task"
      v-on:todo-update="handleTodoUpdate"
      v-on:todo-delete="handleTodoDelete"
    />
  </div>
</template>

<script>
/* eslint-disable no-console */
import TodoItem from "./components/TodoItem";
import AddTodoItem from "./components/AddTodoItem";

export default {
  name: "app",
  data: function() {
    const todoDataString = localStorage.getItem("todoData");
    let todoData;
    if (todoDataString) {
      try {
        todoData = JSON.parse(todoDataString);
      } catch (error) {
        console.warn("Error parsing TODO items from localstorage, ignoring...");
      }
    }
    if (!todoData) {
      todoData = {
        todos: {},
        todo_ids: [],
        lastId: 0
      };
    }
    return todoData;
  },
  components: { TodoItem, AddTodoItem },
  methods: {
    updateLocalstorage: function() {
      localStorage.setItem(
        "todoData",
        JSON.stringify({
          todos: this.todos,
          todo_ids: this.todo_ids,
          lastId: this.lastId
        })
      );
    },
    handleTodoUpdate: function(todoId, todoData) {
      this.todos[todoId] = { ...this.todos[todoId], ...todoData };
      this.updateLocalstorage();
    },
    handleTodoAdd: function(todoData) {
      this.lastId = this.lastId + 1;
      this.todos[this.lastId] = {
        ...todoData,
        isCompleted: false,
        createdAd: new Date()
      };
      this.todo_ids = this.todo_ids.concat([this.lastId]);
      this.updateLocalstorage();
    },
    handleTodoDelete: function(todoId) {
      this.todo_ids = this.todo_ids.filter(i => i !== todoId);
      delete this.todos[todoId];
      this.updateLocalstorage();
    }
  }
};
</script>
