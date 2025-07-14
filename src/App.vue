<script setup> 
  import { reactive } from 'vue';

  const estado = reactive({
    filtro: "todas",
    tarefaTemporaria: "",
    tarefas: [
      {
        titulo: "Estudar ES6",
        finalizada: false,
      },
      {
        titulo: "Estudar SASS",
        finalizada: false,
      },
      {
        titulo: "Criar Finite State Machines",
        finalizada: true, 
      }
    ]
  })

function getTarefasPendentes(){
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

function getTarefasFinalizadas(){
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

function getTarefasFiltradas(){
  const filtro = estado.filtro;

  switch (filtro){
    case "pendentes":
      return getTarefasPendentes();
    case "finalizadas":
      return getTarefasFinalizadas();
    default: 
      return estado.tarefas;
  }
}

function cadastraTarefa(){
  const tarefaNova = {
    titulo: estado.tarefaTemporaria,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemporaria = "";
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>
        Minhas Tarefas
      </h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
  </div>
  <div class="container">
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemporaria" @change="evento => estado.tarefaTemporaria = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.finalizada}" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>
<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
