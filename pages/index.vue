<!-- pages/index.vue -->
<template>
  <div class="page">
    <ScratchCard />
  </div>
</template>

<script setup lang="ts">
import ScratchCard from "~/components/ScratchCard.vue";
</script>

<style scoped>
.page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #000; /* Black background */
  color: #fff;
  text-align: center;
  padding: 20px;
}

.prize-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 10px;
}

h1 {
  font-size: 2rem;
  color: #ff5722;
}

p {
  font-size: 1rem;
  color: #757575;
}
</style>
