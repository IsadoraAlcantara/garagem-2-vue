<script setup>
import { onMounted } from 'vue'
import { PassageUser } from '@passageidentity/passage-elements/passage-user'
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()
</script>

<template>
  <header>
    <nav>
      <router-link :to="{ name: 'home' }">Home</router-link>
      <router-link to="/categorias">Categorias</router-link>
      <router-link to="/acessorios">Acessórios</router-link>
      <router-link to="/cores">Cores</router-link>
      <router-link to="/marcas">Marcas</router-link>
      <router-link to="/modelos">Modelos</router-link>
      <router-link to="/veiculos">Veiculos</router-link>
      <div v-if="authStore.loggedIn">
        <router-link to="/logout">Logout</router-link>
        {{ authStore.user.email }}
      </div>
      <router-link v-else to="/login">Login</router-link>
    </nav>
  </header>
  <main>
    <RouterView />
  </main>
</template>
<style scoped>
nav {
  display: flex;
  gap: 2vw;
  justify-content: center;
}

nav a {
  color: #797E8C;
}

nav a:hover {
  color: #181B59;
}
</style>
