<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';  // Assumindo que está usando axios para requisições

// Estado reativo para armazenar os dados do usuário
const usuario = ref(null);

// Função para formatar a data e hora
const formatDate = (dateString) => {
  if (!dateString) return null;
  const date = new Date(dateString);
  return date.toLocaleString('pt-BR', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
  });
};

// Função para buscar as informações do usuário logado
const getUserData = async () => {
  try {
    const response = await axios.get('/usuarios/me');
    usuario.value = response.data;  // Armazena os dados do usuário
  } catch (error) {
    console.error('Erro ao buscar dados do usuário:', error);
  }
};

// Chama a função para buscar os dados quando o componente é montado
onMounted(() => {
  getUserData();
});
</script>

<template>
  <div v-if="usuario">
    <h1>Informações do Usuário</h1>
    <div class="user-info">
      <img v-if="usuario.foto && usuario.foto.url" :src="usuario.foto.url" alt="Foto do usuário" class="user-photo" />
      <img v-else src="https://via.placeholder.com/150" alt="Sem foto" class="user-photo" />

      <p><strong>Nome:</strong> {{ usuario.name }}</p>
      <p><strong>Email:</strong> {{ usuario.email }}</p>
      <p><strong>ID:</strong> {{ usuario.id }}</p>
      <p><strong>Superuser:</strong> {{ usuario.is_superuser ? 'Sim' : 'Não' }}</p>
      <p><strong>Ativo:</strong> {{ usuario.is_active ? 'Sim' : 'Não' }}</p>
      <p><strong>Staff:</strong> {{ usuario.is_staff ? 'Sim' : 'Não' }}</p>
      <p><strong>Último Login:</strong> {{ formatDate(usuario.last_login) || 'Nunca logado' }}</p>
      <p><strong>Grupos:</strong> {{ usuario.groups.join(', ') }}</p>
    </div>
  </div>
  <div v-else>
    <p>Carregando informações do usuário...</p>
  </div>
</template>

<style>
.user-info {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  max-width: 500px;
  margin: 0 auto;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.user-photo {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  display: block;
  margin: 0 auto 20px;
}

p {
  font-size: 1.1rem;
  margin-bottom: 10px;
}
</style>
