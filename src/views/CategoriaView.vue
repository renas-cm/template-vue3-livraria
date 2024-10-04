<script setup>
import { ref, reactive, onMounted } from "vue";
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();

const defaultCategoria = { id: null, descricao: "" };
const categorias = ref([]);
const categoria = reactive({ ...defaultCategoria });

onMounted(async () => {
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
});

function limpar() {
  Object.assign(categoria, { ...defaultCategoria });
}

async function salvar() {
  if (categoria.id) {
    await categoriasApi.atualizarCategoria(categoria);
  } else {
    await categoriasApi.adicionarCategoria(categoria);
  }
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}

function editar(categoria_para_editar) {
  Object.assign(categoria, categoria_para_editar);
}

async function excluir(id) {
  await categoriasApi.excluirCategoria(id);
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}
</script>

<template>
  <h1>Categoria</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="categoria.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="categoria in categorias" :key="categoria.id">
      <span @click="editar(categoria)">
        ({{ categoria.id }}) - {{ categoria.descricao }} -
      </span>
      <button @click="excluir(categoria.id)">X</button>
    </li>
  </ul>
</template>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  margin: 0;
  padding: 20px;
  color: #333;
}

h1 {
  text-align: center;
  color: #4a90e2;
  font-size: 2rem;
}

hr {
  border: none;
  height: 1px;
  background-color: #ddd;
  margin: 20px 0;
}

.form {
  display: flex;
  gap: 10px;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 300px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s;
}

input[type="text"]:focus {
  border-color: #4a90e2;
  outline: none;
}

button {
  padding: 10px 15px;
  background-color: #4a90e2;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #357ab7;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: white;
  padding: 15px;
  margin: 10px 0;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li span {
  cursor: pointer;
}

li span:hover {
  color: #4a90e2;
}

button:hover {
  background-color: #e74c3c;
}
</style>
