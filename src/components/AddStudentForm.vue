<template>
  <div class="card">
    <h3>Add New Student</h3>
        <!-- TODO: form with @submit.prevent that calls submit() , using v-if instead -->
     <!-- TODO: toggle button to hide or show the form -->
    <!-- TODO: v-model inputs for name, email -->
    <!-- TODO: v-model select for grade -->
    <!-- TODO: v-model.number input for courseId -->
    <!-- TODO: submit button -->
    <button @click="showForm = !showForm" class="toggle-btn" type="button">
      {{ showForm ? 'Hide Form' : 'Show Form' }}
    </button>
    <form v-if="showForm" @submit.prevent="submit">
      <input type="text" v-model="name" placeholder="Name" required />
      <input type="email" v-model="email" placeholder="Email" required />
      <select v-model="grade" required>
        <option value="" disabled>Select Grade</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
        <option>D</option>
        <option>F</option>
      </select>
      <select id="student-course" v-model.number="courseId" required>
        <option value="" disabled>Select a course</option>
        <option v-for="course in courses" :key="course.id" :value="course.id">
          {{ course.code }} - {{ course.name }}
        </option>
      </select>
      <button type="submit" class="btn-add">Add Student</button>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    courses: {
      type: Array,
      required: true
    }
  },

  emits: ['student-added'],

  data() {
    return {
      // TODO: local form state — name, email, grade, courseId
        name: "",
        email: "",
        grade: "A",
        courseId: null,
        showForm: false
    }
  },

  watch: {
    courses: {
      immediate: true,
      handler(newCourses) {
        if (!this.courseId && newCourses.length > 0) {
          this.courseId = newCourses[0].id;
        }
      }
    }
  },

  methods: {
    // TODO: submit()
    // Emit 'student-added' with { name, email, grade, course: this.courseId }
    // Clear the form fields after emitting
    async submit() {
      this.$emit('student-added', { name: this.name, email: this.email, grade: this.grade, course: this.courseId })
      // Clear the form fields after emitting
      this.name = ""
      this.email = ""
      this.grade = "A"
      this.courseId = this.courses.length > 0 ? this.courses[0].id : null // Reset to first course if available
      this.showForm = false // Hide the form after submission
    }

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
input, select {
  display: block;
  width: 50%;
  padding: 10px 12px;
  margin-bottom: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
}

button {
  border: none;
  border-radius: 10px;
  padding: 10px 14px;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.15s ease, box-shadow 0.2s ease, background-color 0.2s ease;
}

button:hover {
  transform: translateY(-1px);
}

button:active {
  transform: translateY(0);
}

.toggle-btn {
  width: 20%;
  margin-bottom: 12px;
  background: #0f766e;
  color: #f0fdfa;
  box-shadow: 0 6px 16px rgba(15, 118, 110, 0.28);
}

.toggle-btn:hover {
  background: #115e59;
}

.btn-add {
  width: 20%;
  background: #15803d;
  color: #f0fdf4;
  box-shadow: 0 6px 16px rgba(21, 128, 61, 0.28);
}

.btn-add:hover {
  background: #166534;
}
</style>
