<template>
  <div class="container mt-4">
    <h1 class="text-center">Lista de Tarefas</h1>
    <ListaTarefas />
  </div>
</template>

<script>
import ListaTarefas from './components/ListaTarefas.vue';

export default {
  components: {
    ListaTarefas,
  },
};
</script>