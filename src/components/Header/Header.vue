<script setup>
import HeaderRight from "./HeaderRight.vue";
</script>

<template>
  <div class="header">
    <img
      alt="Vue logo"
      class="logo"
      src="./../../assets/provided-icons/Logo.png"
    />
    <HeaderRight />
  </div>
</template>

<style scoped>
.header {
  display: flex;
  background-color: white;
  justify-content: space-between;
  align-items: center;
  padding: 16px 10%;
}

.logo {
  width: 170px;
  height: 56px;
}
</style>
