<!-- HTML -->
<template>
  <h1>Profile</h1>
  <button @click="fetchData">teste</button>

  <div class="col-3 p-3 mt" v-for="(curso, index) in cursos" :key="index">
    <h2>{{ curso.nome }}</h2>
    <!-- Adicione mais campos do curso, se necessário -->
  </div>
</template>

<!-- JavaScript -->
<script setup>
import axios from "axios";
import { reactive } from "vue";

class UseApi {
  constructor(urlBase) {
    this.urlBase = urlBase;
  }

  async getAll() {
    try {
      const response = await axios.get(this.urlBase);
      return response.data.content; // Retorna os dados da resposta
    } catch (error) {
      console.error("Erro ao buscar dados:", error);
      throw error; // Lança o erro para ser tratado por quem chamar o método
    }
  }
}

const cursos = reactive([]);
const apiClient = new UseApi("http://localhost:8080/curso/list");

// Função para ser chamada no evento de clique do botão
async function fetchData() {
  try {
    const data = await apiClient.getAll();
    cursos.splice(0, cursos.length, ...data); // Atualiza cursos com os novos dados
    console.log("Cursos atualizados:", cursos);
  } catch (error) {
    console.error("Erro ao buscar cursos:", error);
  }
}
</script>

<!-- CSS -->
<style scoped></style>
