<template>
  <div class="container mt-4">
    <h1 class="text-center">TODO List</h1>
    <TaskList />
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  components: {
    TaskList,
  },
};
</script>