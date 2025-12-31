<script setup lang="ts">
import { ref } from "vue";

const formToggle = ref(false);

function toggleVisibleForm() {
  formToggle.value = !formToggle.value;
}
</script>

<template>
  <div class="container">
    <!-- action buttons -->
    <div class="actions">
      <button :class="{ active: !formToggle }" @click="toggleVisibleForm">
        Login
      </button>
      <button :class="{ active: formToggle }" @click="toggleVisibleForm">
        Sign up
      </button>
    </div>

    <!-- login form -->
    <div v-if="!formToggle" class="login-form">
      <h3 class="title">Welcome back</h3>

      <form class="inputs">
        <div>
          <label for="email-field">Email:</label>
          <input type="email" name="" id="email-field" />
        </div>

        <div>
          <label for="password-field">Password:</label>
          <input type="password" name="password" id="password-field" />
        </div>

        <button type="submit">Login</button>
      </form>
    </div>

    <!-- sign up form -->
    <div v-else class="sign-up-form">
      <h3>Sign up to join us</h3>
      <form class="inputs">
        <div>
          <label for="fullname">Full Name:</label
          ><input type="text" name="fullname" id="fullname" />
        </div>

        <div>
          <label for="email-field">Email:</label
          ><input type="text" name="email-field" id="email-field" />
        </div>

        <div>
          <label for="password-field">Password:</label>
          <input type="password" name="password" id="password-field" />
        </div>

        <div>
          <label for="password-field">Confirm Password:</label>
          <input type="password" name="password" id="password-field" />
        </div>

        <button type="submit">Register</button>
      </form>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 1px 2px 3px #cfecec;
}

button {
  border: none;
  background: unset;
  cursor: pointer;
}

.actions {
  max-width: 80%;
  margin-inline: auto;
  /* padding-block: 5px; */
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  background-color: #cfecec;
}

.actions button {
  padding-block: 10px;
  width: 50%;
}

.actions button.active {
  color: white;
  background-color: black;
}

h3 {
  text-align: center;
}

label,
input,
button {
  display: block;
}
</style>
