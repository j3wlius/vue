<script setup lang="ts">
import { ref, watch } from "vue";

const isSubmitted = ref(false);

// login form data
const loginFormData = ref({
  email: "",
  password: "",
});

// register form data
const registerFormData = ref({
  fullName: "",
  email: "",
  password: "",
  confirm_password: "",
});

// errors data
const loginFormErr = ref({
  emailError: "",
  passwordError: "",
});

const registerFormErr = ref({
  nameErr: "",
  passwordErr: "",
  confirmPasswordErr: "",
  emailErr: "",
});

// persist selected tab using watch
const savedTab = sessionStorage.getItem("authTab");
const formToggle = ref(savedTab === "signup");
watch(formToggle, (value) => {
  sessionStorage.setItem("authTab", value ? "signup" : "login");
});

// form toggle function between login and sign up
function toggleVisibleForm() {
  formToggle.value = !formToggle.value;
}

// mock login function
function mockLogin(email: string, password: string) {
  return new Promise<{ token: string }>((resolve, reject) => {
    setTimeout(() => {
      if (email == "test@email.com" && password == "password123") {
        resolve({ token: "fake-jwt-token" });
      } else {
        reject(new Error("Invalid login credentials"));
      }
    }, 1500);
  });
}

// login handler
async function handleLogin() {
  isSubmitted.value = true;

  loginFormErr.value.emailError = validateEmail(loginFormData.value.email);

  loginFormErr.value.passwordError = checkifPasswordNotEmpty(
    loginFormData.value.password
  );

  if (loginFormErr.value.emailError || loginFormErr.value.passwordError) return;

  try {
    const response = await mockLogin(
      loginFormData.value.email,
      loginFormData.value.password
    );

    console.log("Logged In", response.token);
  } catch (e: any) {
    console.log(e.message);
  }
}

// mock registration
function mockRegistration(
  fullname: string,
  email: string,
  password: string,
  confirm_password: string
) {
  return new Promise<{ message: string }>((resolve, reject) => {
    setTimeout(() => {
      if (
        fullname === "New User" &&
        email === "test@email.com" &&
        password === "Password123!" &&
        confirm_password === "Password123!"
      ) {
        resolve({ message: "User created successfully" });
      } else {
        reject(new Error("Something is wrong"));
      }
    }, 1500);
  });
}

// registration handler
async function handleRegister() {
  isSubmitted.value = true;

  nameValidation(registerFormData.value.fullName);

  registerFormErr.value.emailErr = validateEmail(registerFormData.value.email);

  registerFormErr.value.passwordErr =
    checkifPasswordNotEmpty(registerFormData.value.password) ||
    checkPasswordStrength(registerFormData.value.password);

  checkPasswordMatch(registerFormData.value.password);

  if (
    registerFormErr.value.nameErr ||
    registerFormErr.value.emailErr ||
    registerFormErr.value.passwordErr ||
    registerFormErr.value.confirmPasswordErr
  )
    return;

  try {
    const response = await mockRegistration(
      registerFormData.value.fullName,
      registerFormData.value.email,
      registerFormData.value.password,
      registerFormData.value.confirm_password
    );

    console.log(response.message);
  } catch (e) {
    console.log("Error", e);
  }
}

// forms validation
// 1. login form validation
function validateEmail(email: string) {
  if (!email.trim()) {
    return "Email is required";
  }

  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(email)) {
    return "Enter a valid email address";
  }

  return "";
}

function checkifPasswordNotEmpty(password: string) {
  if (!password.trim()) {
    return "Password is required";
  }
  return "";
}

// 2. register form validation
function nameValidation(name: string) {
  const trimmedName = name.trim();

  if (!trimmedName) {
    return (registerFormErr.value.nameErr = "Name is required");
  }

  if (trimmedName.length < 3) {
    return (registerFormErr.value.nameErr =
      "Name has to atleast 3 characters long");
  }
  return (registerFormErr.value.nameErr = "");
}

// password strength checker
function checkPasswordStrength(password: string) {
  if (password.length < 8)
    return "Password too short. Should be atleast 8 characters";

  if (!(/[a-z]/.test(password) || /[A-Z]/.test(password)))
    return "Password should contain at least a lowercase or uppercase alphabet";

  if (!/\d/.test(password)) return "Password should contain at least one 0-9";

  if (!/[@$!%*?&]/.test(password))
    return "Password should contain atleast 1 special character";

  return "";
}

// check if passwords match
function checkPasswordMatch(password: string) {
  if (
    !registerFormErr.value.passwordErr &&
    !(password === registerFormData.value.confirm_password)
  ) {
    return (registerFormErr.value.confirmPasswordErr =
      "Passwords do not match");
  }

  return "";
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

      <form
        class="inputs"
        method="post"
        @submit.prevent="handleLogin"
        novalidate
      >
        <div>
          <label for="email-field">Email:</label>
          <input
            type="email"
            name=""
            id="email-field"
            v-model="loginFormData.email"
            required
            @input="isSubmitted && (loginFormErr.emailError = '')"
            :class="{ error: isSubmitted && loginFormErr.emailError }"
          />
          <p class="error">{{ loginFormErr.emailError }}</p>
        </div>

        <div>
          <label for="password-field">Password:</label>
          <input
            type="password"
            name="password"
            id="password-field"
            v-model="loginFormData.password"
            required
            @input="isSubmitted && (loginFormErr.passwordError = '')"
            :class="{ error: isSubmitted && loginFormErr.passwordError }"
          />
          <p class="error">{{ loginFormErr.passwordError }}</p>
        </div>

        <button type="submit">Login</button>
      </form>
    </div>

    <!-- sign up form -->
    <div v-else class="sign-up-form">
      <h3>Sign up to join us</h3>
      <form class="inputs" method="post" @submit.prevent="handleRegister">
        <div>
          <label for="fullname">Full Name:</label
          ><input
            type="text"
            name="fullname"
            id="fullname"
            :class="{ error: isSubmitted && registerFormErr.nameErr }"
            v-model="registerFormData.fullName"
            @input="
              {
                isSubmitted && (registerFormErr.nameErr = '');
              }
            "
          />
          <p class="error">{{ registerFormErr.nameErr }}</p>
        </div>

        <div>
          <label for="email-field">Email:</label
          ><input
            type="text"
            name="email-field"
            id="email-field"
            :class="{ error: isSubmitted && registerFormErr.emailErr }"
            v-model="registerFormData.email"
            @input="
              {
                isSubmitted && (registerFormErr.emailErr = '');
              }
            "
          />
          <p class="error">{{ registerFormErr.emailErr }}</p>
        </div>

        <div>
          <label for="password-field">Password:</label>
          <input
            type="password"
            name="password"
            id="password-field"
            :class="{ error: isSubmitted && registerFormErr.passwordErr }"
            @input="
              {
                isSubmitted && (registerFormErr.passwordErr = '');
              }
            "
            v-model="registerFormData.password"
          />
          <p class="error">{{ registerFormErr.passwordErr }}</p>
        </div>

        <div>
          <label for="password-field">Confirm Password:</label>
          <input
            type="password"
            name="password"
            id="password-field"
            :class="{
              error:
                (isSubmitted && registerFormErr.confirmPasswordErr) ||
                registerFormErr.passwordErr,
            }"
            @input="
              {
                isSubmitted && (registerFormErr.confirmPasswordErr = '');
              }
            "
            v-model="registerFormData.confirm_password"
          />
          <p class="error" v-if="registerFormErr.confirmPasswordErr">
            {{ registerFormErr.confirmPasswordErr }}
          </p>
        </div>

        <button type="submit">Register</button>
      </form>
    </div>
  </div>
</template>

<style scoped>
/* -------- Root container -------- */
.container {
  width: 420px;
  margin: 2rem auto;
  padding: 2rem;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08), 0 2px 8px rgba(0, 0, 0, 0.04);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, sans-serif;
}

/* -------- Action toggle buttons -------- */
.actions {
  display: flex;
  background: #f1f5f9;
  border-radius: 12px;
  padding: 4px;
  margin-bottom: 2rem;
  width: 90%;
}

.actions button {
  flex: 1;
  padding: 0.75rem 0;
  border-radius: 10px;
  font-weight: 600;
  font-size: 0.9rem;
  color: #475569;
  background: transparent;
  border: none;
  cursor: pointer;
  transition: background-color 0.25s ease, color 0.25s ease,
    box-shadow 0.25s ease;
}

.actions button.active {
  background: #0f172a;
  color: #ffffff;
  box-shadow: 0 4px 12px rgba(15, 23, 42, 0.25);
}

/* -------- Titles -------- */
h3 {
  text-align: center;
  font-size: 1.4rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  color: #0f172a;
}

/* -------- Forms -------- */
.inputs {
  max-width: 90%;
  display: grid;
  /* gap: 1.25rem; */
}

/* -------- Labels -------- */
label {
  /* font-size: 0.8rem; */
  font-weight: 600;
  color: #475569;
  margin-bottom: 0.5rem;
  display: block;
}

/* -------- Inputs -------- */
input {
  width: 100%;
  padding: 0.7rem 0.75rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  background: #f8fafc;
  font-size: 0.9rem;
  transition: border-color 0.25s ease, box-shadow 0.25s ease,
    background-color 0.25s ease;
}

input.error {
  box-shadow: 0 0 0 3px crimson;
}

input:focus {
  outline: none;
  background: #ffffff;
  box-shadow: 0 0 0 3px rgba(15, 23, 42, 0.15);
}

/* -------- Submit buttons -------- */
.inputs button {
  margin-top: 0.5rem;
  padding: 0.8rem;
  border-radius: 12px;
  border: none;
  background: linear-gradient(135deg, #0f172a, #1e293b);
  color: #ffffff;
  font-weight: 700;
  font-size: 0.95rem;
  cursor: pointer;
  transition: transform 0.15s ease, box-shadow 0.15s ease, opacity 0.15s ease;
}

.inputs button:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 18px rgba(15, 23, 42, 0.25);
}

.inputs button:active {
  transform: translateY(0);
  opacity: 0.9;
}

.error {
  color: rgb(223, 16, 57);
  margin-top: 0;
}
/* -------- Small screens -------- */
@media (max-width: 480px) {
  .container {
    padding: 1.5rem;
  }
}
</style>
