<template>
  <!-- TODO: display student name, email, grade -->
  <!-- TODO: delete button that emits 'delete' with student.id -->
  <div class="card">

  </div>
</template>

<script>
export default {
  props: {
    // TODO: define student prop (Object, required)
  },

  emits: ['delete']
}
</script>

<style scoped>
.card {
  background: white;
  padding: 16px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

/* TODO: style the card content and delete button */
</style>
