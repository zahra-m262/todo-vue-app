<template>
  <div id="app" class="container text-center mt-5">
    <h1 class="mb-3">To Do</h1>

    <p>Completed Todos {{ completedTodos }}</p>

    <todo-form @onNewTodo="handleNewTodo"> </todo-form>
    <todo-list-items
      v-for="todo in todoList"
      :key="todo.id"
      :todo="todo"
      @onToggleIsChecked="handleToggleIsChecked"
      @onToggleEditTodo="handleToggleEditTodo"
      @onUpdateTodo="handleUpdateTodo"
      @onDeleteTodo="handleDeleteTodo"
    ></todo-list-items>
  </div>
</template>

<script>
import TodoForm from './components/TodoForm.vue';
import TodoListItems from './components/TodoListItems.vue';

export default {
  name: 'App',
  components: {
    TodoForm,
    TodoListItems,
  },

  data() {
    return {
      todoList: [],
    };
  },
  computed: {
    completedTodos() {
      return this.todoList.filter((todo) => todo.isChecked).length;
    },
  },

  methods: {
    handleNewTodo(newTodo) {
      this.todoList.push(newTodo);

      console.log(this.todoList);
    },
    handleToggleIsChecked({ value, id }) {
      const todo = this.findTodo(id);
      todo.isChecked = value;
    },

    handleToggleEditTodo(id) {
      const todo = this.findTodo(id);
      todo.editMode = !todo.editMode;
    },

    findTodo(id) {
      return this.todoList.find((todo) => todo.id === id);
    },

    handleUpdateTodo(updatedTodo) {
      this.todoList = this.todoList.map((todo) => {
        if (todo.id === updatedTodo.id) {
          return updatedTodo;
        }
        return todo;
      });
    },

    handleDeleteTodo(id) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
  },
};
</script>
