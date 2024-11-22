<template>
    <b-modal
      :title="tarefa ? 'Editar Tarefa' : 'Criar Tarefa'"
      @hide="fecharModal"
      v-model="exibir"
      hide-footer>
      <div>
        <form @submit.prevent="salvarTarefa">
          <div class="mb-3">
            <label for="description" class="form-label">Descrição</label>
            <input
              id="descricao"
              type="text"
              class="form-control"
              v-model="form.descricao"
              required
            />
          </div>
          <div class="mb-3">
            <label for="deadline" class="form-label">Prazo</label>
            <input
              id="prazo"
              type="date"
              class="form-control"
              v-model="form.prazo"
              required
            />
          </div>
          <button class="btn btn-primary" type="submit">Salvar</button>
          <button class="btn btn-secondary" @click="fecharModal" type="button">Cancelar</button>
        </form>
      </div>
    </b-modal>
  </template>
  
  <script>
  import axios from 'axios';

  export default {
    props: ['tarefa'],
    data() {
      return {
        exibir: true,
        form: {
          descricao: '',
          prazo: '',
        },
      };
    },
    watch: {
      tarefa: {
        immediate: true,
        handler(novaTarefa) {
          if (novaTarefa) {
            this.form = { ...novaTarefa };
          } else {
            this.form = {
              descricao: '',
              prazo: '',
            };
          }
        },
      },
    },
    methods: {
      salvarTarefa() {
        const metodoReq = this.tarefa ? 'put' : 'post';
        const url = this.tarefa
          ? `http://localhost:3000/tarefas/${this.tarefa.id}`
          : 'http://localhost:3000/tarefas';
  
        axios[metodoReq](url, this.form).then(() => {
          this.$emit('salvar');
          this.fecharModal();
        });
      },
      fecharModal() {
        this.$emit('fechar');
      },
    },
  };
  </script>
  