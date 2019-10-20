<template>
    <div>
        <input v-model="inputValue" />
        <button v-on:click="onAddTodo">Add</button>
    </div>
</template>

<script>
export default {
    name: 'AddTodoItem',
    data: function () {
        return { inputValue: '' };
    },
    methods: {
        onAddTodo: function () {
            this.$emit('todo-add', { task: this.inputValue });
            this.inputValue = '';
        },
    },
};
</script>
