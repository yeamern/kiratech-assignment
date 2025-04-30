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
  background-color: #ffffff;
  justify-content: space-between;
  align-items: center;
  padding: 16px 10%;
  box-shadow: 0px 2px 5px 0px #0000001a;
}

.logo {
  width: 170px;
  height: 56px;
}
</style>
