<script setup>
  import { reactive } from 'vue';
  import Header from './components/Header.vue';
  import Form from './components/Form.vue';
  import Todolist from './components/Todolist.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar inglês',
        finalizada: false,
      },
      {
        titulo: 'Ir para academia',
        finalizada: true,
      },
    ]
  })

  
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }
  
  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
        default:
          return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Header :tarefas-pendentes="getTarefasPendentes().length" />
    <Form :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefaTemp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value", :cadastra-tarefa="cadastraTarefa" />
    <Todolist :tarefas="getTarefasFiltradas()" />
  </div>
</template>

