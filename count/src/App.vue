<script setup>
  import {ref, computed} from "vue"

  const count = ref(2)

  const computedValue = computed(() => {
    return count.value * 2
  })

  function computeValue(){
    count.value = count.value * 2
  }
</script>

<template>
  <h1>{{ computedValue }}</h1>
  <button @click="computeValue">Double Count</button>
</template>

<style scoped></style>
