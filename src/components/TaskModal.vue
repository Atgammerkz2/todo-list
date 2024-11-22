<template>
    <b-modal
      :title="task ? 'Edit Task' : 'New Task'"
      @hide="closeModal"
      v-model="show"
      hide-footer
    >
      <div>
        <form @submit.prevent="saveTask">
          <div class="mb-3">
            <label for="description" class="form-label">Description</label>
            <input
              id="description"
              type="text"
              class="form-control"
              v-model="form.description"
              required
            />
          </div>
          <div class="mb-3">
            <label for="deadline" class="form-label">Deadline</label>
            <input
              id="deadline"
              type="date"
              class="form-control"
              v-model="form.deadline"
              required
            />
          </div>
          <button class="btn btn-primary" type="submit">Save</button>
          <button class="btn btn-secondary" @click="closeModal" type="button">Cancel</button>
        </form>
      </div>
    </b-modal>
  </template>
  
  <script>
  import axios from 'axios';

  export default {
    props: ['task'],
    data() {
      return {
        show: true,
        form: {
          description: '',
          deadline: '',
        },
      };
    },
    watch: {
      task: {
        immediate: true,
        handler(newTask) {
          if (newTask) {
            this.form = { ...newTask };
          } else {
            this.form = {
              description: '',
              deadline: '',
            };
          }
        },
      },
    },
    methods: {
      saveTask() {
        const method = this.task ? 'put' : 'post';
        const url = this.task
          ? `http://localhost:3000/tasks/${this.task.id}`
          : 'http://localhost:3000/tasks';
  
        axios[method](url, this.form).then(() => {
          this.$emit('save');
          this.closeModal();
        });
      },
      closeModal() {
        this.$emit('close');
      },
    },
  };
  </script>
  