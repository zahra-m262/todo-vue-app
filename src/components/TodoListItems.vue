<template>
  <div class="container mt-4 text-center">
    <div v-if="!editMode" class="d-flex justify-content-center align-items-center">
      {{ isChecked }}
      <input class="form-check-input m-2" type="checkbox" v-model="isChecked" />
      <span>
        <strong>{{ todo.title }}</strong>
      </span>
      <span class="m-2">{{ todo.description }}</span>
      <button class="btn btn-info rounded-1" @click="toggleEditTodo">Edit</button>
      <button class="btn btn-danger rounded-1 m-1" @click="deleteTodo">Delete</button>
    </div>
    <div v-else>
      <TodoForm @onUpdateTodo="handleUpdateTodo" :todo="todo" isEditMode />
    </div>
  </div>
</template>

<script>
import TodoForm from './TodoForm.vue';

export default {
  name: 'TodoListItems',
  components: { TodoForm },
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
  computed: {
    isChecked: {
      get() {
        return this.todo.isChecked;
      },
      set(value) {
        this.$emit('onToggleIsChecked', {
          value,
          id: this.todo.id,
        });
      },
    },

    editMode() {
      return this.todo.editMode;
    },
  },
  methods: {
    toggleEditTodo() {
      this.$emit('onToggleEditTodo', this.todo.id);
    },

    handleUpdateTodo(updatedTodo) {
      this.$emit('onUpdateTodo', updatedTodo);
    },

    deleteTodo() {
      this.$emit('onDeleteTodo', this.todo.id);
    },
  },
};
</script>
