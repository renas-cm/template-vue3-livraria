<script setup>
import { onMounted } from 'vue';
import { PassageUser } from '@passageidentity/passage-elements/passage-user';
import { useAuthStore } from '@/stores/auth';

const authStore = useAuthStore();

</script>

<template>
  <header>
    <router-link :to="{ name: 'home' }">Home</router-link> |
    <router-link to="/categorias">Categorias</router-link> |
    <div v-if="authStore.loggedIn">
      <router-link to="/usuario">Perfil</router-link> |
      <router-link to="/logout">Logout</router-link> |
      {{ authStore.user.email }}
    </div>
    <router-link v-else to="/login">Login</router-link>
  </header>
  <main>
    <RouterView />
  </main>
</template>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  margin: 0;
  padding: 0;
  color: #333;
}

header {
  background-color: #4a90e2;
  padding: 5px;
  text-align: center;
  color: white;
  font-size: 1.2rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

header a {
  color: white;
  text-decoration: none;
  margin: 0 15px;
  transition: color 0.3s;
}

header a:hover {
  color: #ddd;
}

header div {
  display: inline;
  margin-left: 0px;
}

main {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
}

main h1 {
  color: #4a90e2;
  text-align: center;
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

@media (max-width: 600px) {
  header {
    font-size: 1rem;
  }

  header a {
    margin: 0 10px;
  }

  main {
    padding: 10px;
  }
}
</style>
