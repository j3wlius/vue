<script lang="ts" setup>
import AuthLayout from "./components/AuthLayout.vue";
import EmailInput from "./components/EmailInput.vue";
</script>

<template>
  <!-- <div class="bg-white">
    <EmailInput />
  </div> -->
  <AuthLayout>
    <h1 class="text-center">Welcome Back</h1>
    <EmailInput />
  </AuthLayout>
</template>
