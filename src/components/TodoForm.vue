<template>
  <div class="container mt-5 text-center">
    <form action="#" @submit.prevent="submit">
        <div class="d-flex justify-content-center">
          <input class="form-control w-25 mx-1" type="text" required placeholder="Title" v-model="title" />
        <input
        class="form-control w-25 "
          type="text"
          required
          placeholder="Description"
          v-model="description"
        />
        </div>
      <button type="submit" class="btn btn-warning rounded-1 m-2">
        {{ isEditMode ? 'Save' : 'Add' }}
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'TodoForm',

  props: {
    todo: {
      type: Object,
      default: () => ({}),
    },
    isEditMode: {
      type: Boolean,
      default: false,
    },
  },

  created() {
    if (this.isEditMode) {
      this.title = this.todo.title;
      this.description = this.todo.description;
      return;
    }
  },

  data() {
    return {
      title: '',
      description: '',
    };
  },
  methods: {
    submit() {
      if (this.isEditMode) {
        this.emitUpdateTodo();
        return;
      }
      this.emitNewTodo();
    },

    emitUpdateTodo() {
      this.$emit('onUpdateTodo', {
        title: this.title,
        description: this.description,
        id: this.todo.id,
        isChecked: this.todo.isChecked,
        editMode: false,
      });
    },

    emitNewTodo() {
      this.$emit('onNewTodo', {
        title: this.title,
        description: this.description,
        id: `todo_${Math.random() * 1000}`,
        isChecked: false,
        editMode: false,
      });

      this.title = '';
      this.description = '';
    },
  },
};
</script>
