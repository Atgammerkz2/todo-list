<template>
    <div>
      <button class="btn btn-primary mb-3 pl-3 pr-3" @click="abrirModal()">
        Adicionar Tarefa
      </button>
  
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Descrição</th>
            <th>Prazo</th>
            <th>Status</th>
            <th>Ações</th>
          </tr>
        </thead>

        <tbody>
          <tr
            v-for="tarefa in tarefas"
            :key="tarefa.id"
            :class="{ 'table-secondary': tarefa.concluida }">

            <td :class="{ 'riscada': tarefa.concluida }">
              {{ tarefa.descricao }}
            </td>
            <td>{{ tarefa.prazo }}</td>
            <td>{{ tarefa.concluida ? 'Concluída' : 'Pendente' }}</td>
            <td>
              <button class="btn btn-success btn-sm" @click="marcarComoConcluida(tarefa)">
                {{ tarefa.concluida ? 'Desfazer' : 'Concluir' }}
              </button>
              <button class="btn btn-warning btn-sm" @click="abrirModal(tarefa)">
                Editar
              </button>
              <button class="btn btn-danger btn-sm" @click="exibirModalExclusao(tarefa)">
                Excluir
              </button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <!-- Modal de criação/edição -->
      <ModalTarefa
        v-if="exibirModal"
        :task="tarefaSelecionada"
        @salvar="obterTarefas"
        @fechar="fecharModal"
      />
  
      
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import ModalTarefa from './ModalTarefa.vue';
  
  export default {
    components: { ModalTarefa },
    data() {
      return {
        tarefas: [],
        exibirModal: false,
        exibirConfirmacaoDelecao: false,
        tarefaSelecionada: null,
        tarefaParaDeletar: null,
      };
    },
    methods: {
      obterTarefas() {
        axios.get('http://localhost:3000/tarefas').then((resposta) => {
          this.tarefas = resposta.data;
        });
      },
      marcarComoConcluida(tarefa) {
        axios
          .patch(`http://localhost:3000/tarefas/${tarefa.id}`, {
            completed: !tarefa.concluida,
          })
          .then(this.obterTarefas);
      },
      deletarTarefa(id) {
        axios.delete(`http://localhost:3000/tarefas/${id}`).then(this.obterTarefas);
      },
      abrirModal(tarefa = null) {
        this.tarefaSelecionada = tarefa;
        this.exibirModal = true;
      },
      fecharModal() {
        this.exibirModal = false;
        this.tarefaSelecionada = null;
      },
      exibirModalExclusao(tarefa) {
        this.tarefaParaDeletar = tarefa;
        this.exibirConfirmacaoDelecao = true;
      },
      fecharModalDelecao() {
        this.tarefaParaDeletar = null;
        this.exibirConfirmacaoDelecao = false;
      },
      confirmarDelecao() {
        if (this.tarefaParaDeletar) {
          this.deletarTarefa(this.tarefaParaDeletar.id);
          this.fecharModalDelecao();
        }
      },
    }
  };
  </script>
  
  <style scoped>
  .riscada {
    text-decoration: line-through;
  }
  </style>
  