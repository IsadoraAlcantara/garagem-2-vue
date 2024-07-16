<script setup>
import { ref, reactive, onMounted } from 'vue'
import AcessoriosApi from '@/api/acessorios'
const acessoriosApi = new AcessoriosApi()

const defaultAcessorio = { id: null, descricao: '' }
const acessorios = ref([])
const acessorio = reactive({ ...defaultAcessorio })

onMounted(async () => {
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios()
})

function limpar() {
  Object.assign(acessorio, { ...defaultAcessorio })
}

async function salvar() {
  if (acessorio.id) {
    await acessoriosApi.atualizarAcessorio(acessorio)
  } else {
    await acessoriosApi.adicionarAcessorio(acessorio)
  }
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios()
  limpar()
}

function editar(acessorio_para_editar) {
  Object.assign(acessorio, acessorio_para_editar)
}

async function excluir(id) {
  await acessoriosApi.excluirAcessorio(id)
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios()
  limpar()
}
</script>

<template>
  <h1>Acessório</h1>
  <div class="container">
    <div class="form">
      <input type="text" v-model="acessorio.descricao" placeholder="Descrição" />
      <button @click="salvar">Salvar</button>
      <button @click="limpar">Limpar</button>
    </div>
    <ul>
      <li v-for="acessorio in acessorios" :key="acessorio.id">
        <span @click="editar(acessorio)"> ({{ acessorio.id }}) - {{ acessorio.descricao }} - </span>
        <button @click="excluir(acessorio.id)">X</button>
      </li>
    </ul>
  </div>
</template>
<style scoped>
h1 {
  left: 45%;
  color: #292d73;
  font-weight: 600;
  margin: 5vw 0 2vw 0;
}

.container {
  display: flex;
  flex-direction: column;
  padding: 1vw;
  background-color: white;
  width: 30vw;
  min-height: 30vw;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  left: 35%;
}

.form {
  display: flex;
  gap: 0.6vw;
  height: 3vw;
}

input {
  padding: 0.8vw;
  border-radius: 5px;
  border: solid #797E8C 1px;
  outline: none;
  width: 30vw;
}

button {
  background-color: #292d73;
  color: white;
  border: none;
  padding: 0.6vw;
  border-radius: 5px;
}

li {
  background-color: white;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  list-style-type: none;
  margin: 1vw;
  border-radius: 4px;
}
</style>
