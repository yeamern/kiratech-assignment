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
header {
  position: sticky;
  top: 0;
  z-index: 10;
}
main {
  margin-bottom: 64px;
}
</style>
