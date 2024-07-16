<script setup>
import { ref, reactive, onMounted } from "vue";
import VeiculosApi from "@/api/veiculo";
const veiculoApi = new VeiculosApi();

const defaultCor = { id: null, nome: "" };
const veiculo = ref([]);
const cor = reactive({ ...defaultCor });

onMounted(async () => {
  veiculo.value = await veiculoApi.buscarTodosOsVeiculos();
});

function limpar() {
  Object.assign(cor, { ...defaultCor });
}

async function salvar() {
  if (cor.id) {
    await veiculoApi.atualizarCor(cor);
  } else {
    await veiculoApi.adicionarCor(cor);
  }
  veiculo.value = await veiculoApi.buscarTodosOsVeiculos();
  limpar();
}

function editar(cor_para_editar) {
  Object.assign(cor, cor_para_editar);
}

async function excluir(id) {
  await veiculoApi.excluirCor(id);
  veiculo.value = await veiculoApi.buscarTodosOsVeiculos();
  limpar();
}
</script>

<template>
  <h1>Cor</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="cor.nome" placeholder="Nome" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="cor in veiculo" :key="cor.id">
      <span @click="editar(cor)">
        ({{ cor.id }}) - {{ cor.nome }} -
      </span>
      <button @click="excluir(cor.id)">X</button>
    </li>
  </ul>
</template>

<style></style>