<template>
  <!-- TODO: display student name, email, grade -->
  <!-- TODO: delete button that emits 'delete' with student.id -->
  <div class="card">
    <h3>{{ student.name }}</h3>
    <p>{{ student.email }}</p>
    <p>Grade: {{ student.grade }}</p>
    <p>Course: {{ student.course.name }}</p>
    <div class="actions">
      <button class="btn-edit" @click ="$emit('edit', student.id)">Edit</button>
      <button class="btn-delete" @click="$emit('delete', student.id)">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // TODO: define student prop (Object, required)
    student: {
      type: Object,
      required: true
    }
  },

  emits: ['delete', 'edit']
}
</script>

<style scoped>
.card {
  background: white;
  padding: 16px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

/* TODO: style the card content and delete button and edit button */
.card h3 {
  margin: 0 0 8px 0;
}
.card p {
  margin: 4px 0;
}

.actions {
  display: flex;
  gap: 10px;
  margin-top: 12px;
}

.actions button {
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: transform 0.15s ease, background-color 0.2s ease;
}

.actions button:hover {
  transform: translateY(-1px);
}

.actions button:active {
  transform: translateY(0);
}

.btn-edit {
  background-color: #0ea5e9;
  color: #f0f9ff;
}

.btn-edit:hover {
  background-color: #0284c7;
}

.btn-delete {
  background-color: #e11d48;
  color: #fff1f2;
}

.btn-delete:hover {
  background-color: #be123c;
}

</style>
