<template>
  <div class="card">
    <h3>Add New Student</h3>
    <!-- TODO: form with @submit.prevent that calls submit() -->
    <!-- TODO: v-model inputs for name, email -->
    <!-- TODO: v-model select for grade -->
    <!-- TODO: v-model.number input for courseId -->
    <!-- TODO: submit button -->
  </div>
</template>

<script>
export default {
  emits: ['student-added'],

  data() {
    return {
      // TODO: local form state — name, email, grade, courseId
    }
  },

  methods: {
    // TODO: submit()
    // Emit 'student-added' with { name, email, grade, course: this.courseId }
    // Clear the form fields after emitting
  }
}
</script>

<style scoped>
.card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

h3 {
  margin-bottom: 12px;
  color: #0f172a;
}

/* TODO: style the form inputs, select, and button */
</style>
