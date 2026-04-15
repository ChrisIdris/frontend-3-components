<template>
  <section>
    <h2>Login</h2>
    <!-- TODO: show error message if loginError is not empty -->
    <!-- TODO: form with @submit.prevent that calls submit() -->
    <!-- TODO: v-model inputs for username and password -->
    <!-- TODO: submit button -->
  </section>
</template>

<script>
export default {
  emits: ['login'],

  data() {
    return {
      // TODO: local form state — username, password, loginError
    }
  },

  methods: {
    // TODO: submit()
    // Emit 'login' with { username: this.username, password: this.password }
    // The parent (App.vue) handles the actual API call
  }
}
</script>

<style scoped>
/* TODO: style the form, inputs, button, error message */
</style>
