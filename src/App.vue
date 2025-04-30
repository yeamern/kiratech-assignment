<script setup>
import Header from "./components/Header/Header.vue";
import Banner from "./components/Banner/Banner.vue";
import UsersList from "./components/Users/UsersList.vue";
</script>

<template>
  <header>
    <Header />
  </header>

  <main>
    <Banner class="banner" />
    <UsersList />
  </main>
</template>

<style scoped>
main {
  margin-bottom: 64px;
}
</style>
