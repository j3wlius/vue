<script lang="ts" setup>
import { ref } from "vue";

const habits = ref<string[]>([]);

const modelValue = ref<string>("");
const error = ref("");

function addHabit() {
  if (modelValue.value.length >= 5) {
    habits.value.push(modelValue.value);
    modelValue.value = "";
  } else {
  }
}
</script>

<template>
  <div class="h-screen flex items-center justify-center">
    <div class="bg-white p-2.5 w-1/3 rounded-lg shadow-md">
      <h1 class="text-3xl text-center">Welcome to habitify</h1>

      <!-- input habits -->
      <div class="flex w-10/12 justify-center items-center mx-auto">
        <input
          placeholder="Add a habit"
          class="focus:outline-none border-b border-b-blue-400 py-2 my-6 w-10/12 pl-1.5"
          v-model="modelValue"
        />

        <button class="w-2/12 cursor-pointer" @click="addHabit">
          <i class="pi pi-plus"></i>
        </button>
      </div>

      <!-- render habits -->
      <div class="border border-zinc-500">
        <div class="flex border-b">
          <p class="w-4/7 font-bold text-center">Habit</p>
          <p class="w-1/7 border-l font-bold text-center">Count</p>
          <p class="w-2/7 border-l font-bold text-center">Actions</p>
        </div>
        <div v-for="(habit, index) in habits" class="flex" :key="index + 1">
          <p class="w-4/7 text-center">{{ habit }}</p>
          <p class="w-1/7 border-l text-center">2</p>
          <p class="w-2/7 border-l text-center">
            <span><i class="pi pi-trash"></i></span>
            <span><i class="pi pi-check"></i></span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
