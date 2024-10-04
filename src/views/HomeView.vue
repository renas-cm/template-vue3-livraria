<script setup>
import { onMounted, ref } from 'vue';
import { PassageUser } from '@passageidentity/passage-elements/passage-user';
import { useAuthStore } from '@/stores/auth';

const authStore = useAuthStore();
const psg_auth_token = ref('');
const copyMessageVisible = ref(false);  // Controle de visibilidade da mensagem

const getUserInfo = async () => {
  try {
    const authToken = localStorage.getItem('psg_auth_token');
    const passageUser = new PassageUser(authToken);
    const user = await passageUser.userInfo(authToken);
    psg_auth_token.value = authToken;
    if (user) {
      await authStore.setToken(authToken);
    } else {
      authStore.unsetToken();
    }
  } catch (error) {
    authStore.unsetToken();
  }
};

const copyToClipboard = () => {
  navigator.clipboard.writeText(psg_auth_token.value).then(() => {
    // Mostrar a mensagem por alguns segundos
    copyMessageVisible.value = true;
    setTimeout(() => {
      copyMessageVisible.value = false;
    }, 2000); // Mensagem some depois de 2 segundos
  }).catch(err => {
    console.error('Erro ao copiar o token: ', err);
  });
};

onMounted(() => {
  getUserInfo();
});
</script>

<template>
  <h2>Passage Auth Token:</h2>
  <button @click="copyToClipboard">Copiar Token</button>
  <!-- Mostrar a mensagem condicionalmente -->
  <p v-if="copyMessageVisible" class="copy-message">Token copiado com sucesso!</p>
  <p>{{ psg_auth_token }}</p>
</template>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  margin: 0;
  padding: 20px;
  color: #333;
}

h2 {
  font-size: 1.8rem;
  color: #4a90e2;
  margin-bottom: 15px;
  text-align: center;
}

p {
  font-size: 1rem;
  color: #555;
  text-align: center;
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: 20px auto;
  word-break: break-all;
}

button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
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

.copy-message {
  text-align: center;
  color: #4CAF50;
  margin-top: 10px;
  font-weight: bold;
  animation: fadeInOut 2s ease-in-out;
}

@keyframes fadeInOut {
  0% { opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { opacity: 0; }
}

@media (max-width: 600px) {
  p {
    padding: 15px;
    max-width: 90%;
  }
}
</style>
