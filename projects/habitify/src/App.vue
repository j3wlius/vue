<script lang="ts" setup>
import { ref } from "vue";

const habits = ref<string[]>(["Test habit"]);
</script>

<template>
  <div class="h-screen flex items-center justify-center">
    <div class="bg-white p-2.5 w-1/3 rounded-lg shadow-md">
      <h1 class="text-3xl text-center">Welcome to habitify</h1>
      <div class="flex w-10/12 justify-center items-center mx-auto">
        <input
          placeholder="Add a habit"
          class="focus:outline-none border-b border-b-blue-400 py-2 my-6 w-10/12 pl-1.5"
        />
        <button class="w-2/12 cursor-pointer">
          <i class="pi pi-plus"></i>
        </button>
      </div>

      
    </div>
  </div>
</template>

<style scoped></style>
