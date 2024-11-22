<template>
    <div>
      <button
        class="btn btn-primary mb-3"
        @click="openModal()"
      >
        Add Task
      </button>
  
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Description</th>
            <th>Deadline</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="task in tasks"
            :key="task.id"
            :class="{ 'table-secondary': task.completed }"
          >
            <td :class="{ 'text-decoration-line-through': task.completed }">
              {{ task.description }}
            </td>
            <td>{{ task.deadline }}</td>
            <td>{{ task.completed ? 'Completed' : 'Pending' }}</td>
            <td>
              <button
                class="btn btn-success btn-sm"
                @click="toggleComplete(task)"
              >
                {{ task.completed ? 'Undo' : 'Complete' }}
              </button>
              <button
                class="btn btn-warning btn-sm"
                @click="openModal(task)"
              >
                Edit
              </button>
              <button
                class="btn btn-danger btn-sm"
                @click="deleteTask(task.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <TaskModal
        v-if="showModal"
        :task="selectedTask"
        @save="fetchTasks"
        @close="closeModal"
      />
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import TaskModal from './TaskModal.vue';
  
  export default {
    components: { TaskModal },
    data() {
      return {
        tasks: [],
        showModal: false,
        selectedTask: null,
      };
    },
    methods: {
      fetchTasks() {
        axios.get('http://localhost:3000/tasks').then((response) => {
          this.tasks = response.data;
        });
      },
      toggleComplete(task) {
        axios
          .patch(`http://localhost:3000/tasks/${task.id}`, {
            completed: !task.completed,
          })
          .then(this.fetchTasks);
      },
      deleteTask(taskId) {
        axios.delete(`http://localhost:3000/tasks/${taskId}`).then(this.fetchTasks);
      },
      openModal(task = null) {
        this.selectedTask = task;
        this.showModal = true;
      },
      closeModal() {
        this.showModal = false;
        this.selectedTask = null;
      },
    },
    mounted() {
      this.fetchTasks();
    },
  };
  </script>
  
  <style scoped>
  .text-decoration-line-through {
    text-decoration: line-through;
  }
  </style>
  