<template>
  <section>
    <h2>Login</h2>
    <!-- TODO: show error message if loginError is not empty -->
    <p v-if="loginError" class="error">{{ loginError }}</p>
    <!-- TODO: form with @submit.prevent that calls submit() -->
    <form @submit.prevent="submit">
      <!-- TODO: v-model inputs for username and password -->
      <input type="text" v-model="username" placeholder="Username" required />
      <input type="password" v-model="password" placeholder="Password" required />
      <!-- TODO: submit button -->
      <button class="btn-login" type="submit">Login</button>
    </form>
  </section>
</template>

<script>
export default {
  emits: ['login'],

  data() {
    return {
      // TODO: local form state — username, password, loginError
        username: "",
        password: "",
        loginError: ""
    }
  },

  methods: {
    // TODO: submit()
    // Emit 'login' with { username: this.username, password: this.password }
    // The parent (App.vue) handles the actual API call
    async submit() {
      this.$emit('login', { username: this.username, password: this.password })
      // Clear the form fields after emitting
      this.username = ""
      this.password = ""
    }
  }
}
</script>

<style scoped>
/* TODO: style the form, inputs, button, error message */
.error {
  color: red;
  margin-bottom: 12px;
}
section {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}
input {
  display: block;
  width: 100%;
  padding: 10px 12px;
  margin-bottom: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
}
form {
  display: flex;
  gap: 12px;
  flex-direction: column;
}

.btn-login {
  border: none;
  border-radius: 10px;
  padding: 10px 14px;
  font-weight: 700;
  background: #1d4ed8;
  color: #eff6ff;
  cursor: pointer;
  box-shadow: 0 6px 16px rgba(29, 78, 216, 0.28);
  transition: transform 0.15s ease, box-shadow 0.2s ease, background-color 0.2s ease;
}

.btn-login:hover {
  background: #1e40af;
  transform: translateY(-1px);
}

.btn-login:active {
  transform: translateY(0);
}
</style>
