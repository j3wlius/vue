<script setup lang="ts">
/* 
    focusing on reactivity model in vue
    - ref & why .value is important.
  */
import { ref, reactive, toRefs } from "vue";

const username = ref("Julius Tamale");

const newStateObj = reactive({ count: 0, preferences: { darkMode: false } });

const { count, preferences } = toRefs(newStateObj);

function increment() {
  // before using toRefs, the count was not updating
  count.value++;
}

function toggleTheme() {
  // when i destructured, the preference was toggling so i do not know why
  preferences.value.darkMode = !preferences.value.darkMode;
  // if i have used ref, i would have used .value on the object: newStateObj.value.preferences.darkMode = !newStateObj.value.preferences.darkMode
}

function changeName() {
  username.value = "Vue Master";
}
</script>

<template>
  <h1>{{ username }}</h1>
  <button @click="changeName">Change Name</button>

  <h2>{{ count }}</h2>
  <button @click="increment">Count + 1</button>

  <h3>
    Theme Preference:
    <span v-if="preferences.darkMode">Dark Mode</span>
    <span v-else>Light Mode</span>
  </h3>
  <button @click="toggleTheme">Toggle Mode</button>
</template>

<style scoped></style>
