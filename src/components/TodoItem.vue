<template>
  <div>
    <input type="checkbox" v-bind:checked="isCompleted" v-on:click="toggleIsCompleted" />
    <span v-if="editing">
      <input v-model="inputValue" />
      <button v-on:click="onSave">Save</button>
    </span>
    <span v-else v-on:click="onEditStart">
      <span>{{ task }}</span>
      <button v-on:click="onDelete">Delete</button>
    </span>
  </div>
</template>

<script>
/* eslint-disable no-console */
export default {
  name: "HelloWorld",
  props: {
    isCompleted: Boolean,
    task: String,
    id: Number
  },
  data: function() {
    return {
      editing: false,
      inputValue: this.task
    };
  },
  methods: {
    onEditStart: function() {
      this.editing = true;
    },
    onSave: function() {
      this.editing = false;
      this.$emit("todo-update", this.id, { task: this.inputValue });
    },
    onDelete: function() {
      this.$emit("todo-delete", this.id);
    },
    toggleIsCompleted: function() {
      this.$emit("todo-update", this.id, { isCompleted: !this.isCompleted });
    }
  }
};
</script>
