<script setup lang="ts">
import { ref } from "vue";
const value = ref("");
</script>

<template>
  <div class="py-16 flex justify-center">
    <div class="max-h-[500] w-2/3 bg-white">
      <h1>Welcome to Habitify</h1>
      <input v-model="value" placeholder="Input tast" />
    </div>
  </div>
</template>
