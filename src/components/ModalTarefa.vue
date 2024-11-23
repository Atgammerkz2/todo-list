<template>
    <div class="backdrop">
      <div class="modal-cadastro p-4">
        <form @submit.prevent="salvarTarefa">
          <div class="mb-3">
            <label for="descricao" class="form-label">Descrição</label>
            <input
              id="descricao"
              type="text"
              class="form-control"
              v-model="form.descricao"
              required
            />
          </div>
          <div class="mb-3">
            <label for="prazo" class="form-label">Prazo</label>
            <input
              id="prazo"
              type="date"
              class="form-control"
              v-model="form.prazo"
              required
            />
          </div>
          <button class="btn btn-primary mr-3" type="submit">Salvar</button>
          <button class="btn btn-secondary" @click="fecharModal" type="button">Cancelar</button>
        </form>
      </div>
    </div>
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
      }
    }
  };
  </script>
  
  <style scoped>
  .backdrop {
    position: fixed;
    background-color: rgba(0, 0, 0, 0.5);
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal-cadastro {
    background-color: white;
    box-shadow: 0px 8px 1px white;
    border-radius: 20px;
  }
  </style>