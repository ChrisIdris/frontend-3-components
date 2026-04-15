<template>
  <div class="app">
    <!-- TODO: use NavBar component -->
    <!-- Pass :username and :is-logged-in as props -->
    <!-- Listen for @logout event -->

    <main>
      <!-- TODO: when NOT logged in, show LoginForm -->
      <!-- Listen for @login event -->

      <!-- TODO: when logged in, show the dashboard -->
      <!-- Include SearchBar, AddStudentForm, and StudentCards -->
    </main>
  </div>
</template>

<script>
// TODO: import your components
// import NavBar from './components/NavBar.vue'
// import LoginForm from './components/LoginForm.vue'
// import SearchBar from './components/SearchBar.vue'
// import AddStudentForm from './components/AddStudentForm.vue'
// import StudentCard from './components/StudentCard.vue'

const API_URL = "http://localhost:8000/api"

export default {
  components: {
    // TODO: register your components here
  },

  data() {
    return {
      // Auth
      isLoggedIn: false,
      username: "",

      // Students
      students: [],
      isLoading: false,
      searchTerm: ""
    }
  },

  computed: {
    // TODO: filteredStudents — filter this.students by this.searchTerm
  },

  methods: {
    // TODO: handleLogin(credentials)
    // - POST to API_URL + "/token/" with credentials.username and credentials.password
    // - Store token in localStorage
    // - Set isLoggedIn = true, username = credentials.username
    // - Call this.loadStudents()

    // TODO: handleLogout()
    // - Clear localStorage
    // - Set isLoggedIn = false, students = []

    // TODO: loadStudents()
    // - GET from API_URL + "/students/" with Authorization header
    // - Set this.students with the response

    // TODO: handleStudentAdded(studentData)
    // - POST to API_URL + "/students/" with studentData
    // - Call this.loadStudents() to refresh

    // TODO: handleDelete(id)
    // - DELETE to API_URL + "/students/" + id + "/"
    // - Remove from this.students

    getToken() {
      return localStorage.getItem("access_token")
    }
  },

  mounted() {
    // TODO: check if token exists in localStorage
    // If yes, set isLoggedIn = true and call this.loadStudents()
  }
}
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif;
  background-color: #f5f7fa;
  color: #333;
  line-height: 1.6;
}

.app {
  min-height: 100vh;
}

main {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px;
}

h2 {
  margin-bottom: 20px;
  color: #0f172a;
}
</style>
