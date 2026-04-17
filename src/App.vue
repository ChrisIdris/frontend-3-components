<template>
  <div class="app">
    <!-- TODO: use NavBar component -->
    <!-- Pass :username and :is-logged-in as props -->
    <!-- Listen for @logout event -->
     <NavBar :username="username" :is-logged-in="isLoggedIn" @logout="handleLogout" />


    <main>
      <!-- TODO: when NOT logged in, show LoginForm -->
      <LoginForm v-if="!isLoggedIn" @login="handleLogin" />

      <!-- TODO: when logged in, show the dashboard -->
      <!-- Include SearchBar, AddStudentForm, and StudentCards -->
      <div v-else>
        <AddStudentForm :courses="courses" @student-added="handleStudentAdded" />
        <EditStudentForm
          v-if="editingStudent"
          :student="editingStudent"
          :courses="courses"
          @student-updated="handleStudentUpdated"
          @cancel="cancelEdit"
        />
        <SearchBar @search="searchTerm = $event" />
        <FilterBar
          :courses="courses"
          @filters-changed="handleFiltersChanged"
        />

        <p v-if="apiError" class="error">{{ apiError }}</p>
        <div v-if="isLoading">Loading students...</div>
        <div v-else>
          <p v-if="filteredStudents.length === 0">No students found.</p>
          <StudentCard 
            v-for="student in filteredStudents" 
            :key="student.id" 
            :student="student" 
            @edit="startEdit"
            @delete="handleDelete(student.id)" 
          />
        </div>
      </div>

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
import NavBar from './components/NavBar.vue'
import LoginForm from './components/LoginForm.vue'
import SearchBar from './components/SearchBar.vue'
import AddStudentForm from './components/AddStudentForm.vue'
import StudentCard from './components/StudentCard.vue'
import FilterBar from './components/FilterBar.vue'
import EditStudentForm from './components/EditStudentForm.vue'

const API_URL = "http://localhost:8000/api"

export default {
  components: {
    NavBar,
    LoginForm,
    SearchBar,
    AddStudentForm,
    StudentCard,
    FilterBar,
    EditStudentForm
  },

  data() {
    return {
      // Auth
      username: "",
      isLoggedIn: false,

      // Students & Courses
      courses: [], // TODO: load courses from API for the AddStudentForm select
      students: [],
      isLoading: false,
      apiError: "",
      editingStudent: null,
      

      //Inputs for AddStudentForm
      newName: "",
      newEmail: "",
      newGrade: "N/A",
      newCourseId: "",

      //Search & Filter
      searchTerm: "",
      filterGrade: "",
      filterCourse: "",

    }
  },

  computed: {
    
    // TODO: filteredStudents — filter this.students by this.searchTerm
    filteredStudents() {
      const term = this.searchTerm.toLowerCase();
      return this.students.filter(student => {
        const matchesSearch =
          term === "" ||
          student.name.toLowerCase().includes(term) ||
          student.email.toLowerCase().includes(term) ||
          student.course?.name?.toLowerCase().includes(term);
        const matchesGrade = !this.filterGrade || student.grade === this.filterGrade;
        const matchesCourse = !this.filterCourse || student.course?.id === Number(this.filterCourse);
        return matchesSearch && matchesGrade && matchesCourse;
      });
    }
  },

  methods: {
    // TODO: handleLogin(credentials)
    // - POST to API_URL + "/token/" with credentials.username and credentials.password
    // - Store token in localStorage
    // - Set isLoggedIn = true, username = credentials.username
    // - Call this.loadStudents()
    async handleLogin({ username, password }) {
      try {
        const response = await fetch(`${API_URL}/token/`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ username, password })
        });
        if (!response.ok) throw new Error("Login failed");
        const data = await response.json();
        this.setTokens(data.access, data.refresh);
        this.isLoggedIn = true;
        this.username = username;
        await this.loadCourses();
        await this.loadStudents();
      } catch (error) {
        console.error(error);
        // Optionally, you can set a loginError message to show in the LoginForm
      }
    },

    // TODO: handleLogout()
    // - Clear localStorage
    // - Set isLoggedIn = false, students = []
    handleLogout() {
      localStorage.removeItem("access_token");
      localStorage.removeItem("refresh_token");
      this.isLoggedIn = false;
      this.students = [];
      this.courses = [];
      this.editingStudent = null;
      this.username = "";
    },

    async loadCourses() {
      try {
        this.apiError = "";
        const response = await this.authFetch(`${API_URL}/courses/`);
        if (!response.ok) throw new Error("Failed to load courses");
        const data = await response.json();
        this.courses = this.normalizeListResponse(data);
      } catch (error) {
        this.apiError = error.message;
        console.error(error);
      }
    },

    // TODO: loadStudents()
    // - GET from API_URL + "/students/" with Authorization header
    // - Set this.students with the response
    async loadStudents() {
      this.isLoading = true;
      try {
        this.apiError = "";
        const response = await this.authFetch(`${API_URL}/students/`);
        if (!response.ok) throw new Error("Failed to load students");
        const data = await response.json();
        const studentList = this.normalizeListResponse(data); // Handle both array and { results: [] } formats due to pagination
        this.students = this.attachCourseObjects(studentList); // Replace course IDs with course objects for easier access in the UI
      } catch (error) {
        this.apiError = error.message;
        console.error(error);
      } finally {
        this.isLoading = false;
      }
    },

    normalizeListResponse(data) {
      if (Array.isArray(data)) return data;
      if (data && Array.isArray(data.results)) return data.results;
      return [];
    },

    // Replace course IDs in student objects with the actual course objects for easier access in the UI - TODO: Change students api to include course details to avoid this extra step
    attachCourseObjects(students) {
      return students.map((student) => {
        if (student.course && typeof student.course === "object") {
          return student;
        }

        const courseObj = this.courses.find((course) => course.id === student.course);
        return {
          ...student,
          course: courseObj || { id: student.course, name: "Unknown course" }
        };
      });
    },

    // TODO: handleStudentAdded(studentData)
    // - POST to API_URL + "/students/" with studentData
    // - Call this.loadStudents() to refresh
    async handleStudentAdded(studentData) {
      try {
        const response = await this.authFetch(`${API_URL}/students/`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(studentData)
        });
        if (!response.ok) throw new Error("Failed to add student");
        await this.loadStudents();
      } catch (error) {
        console.error(error);
      }
    },

    startEdit(studentId) {
      const student = this.students.find((item) => item.id === studentId);
      if (!student) return;

      this.editingStudent = {
        id: student.id,
        name: student.name,
        email: student.email,
        grade: student.grade,
        course: student.course?.id ?? ""
      };
    },

    cancelEdit() {
      this.editingStudent = null;
    },

    async handleStudentUpdated(studentData) {
      try {
        const response = await this.authFetch(`${API_URL}/students/${studentData.id}/`, {
          method: "PATCH",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            name: studentData.name,
            email: studentData.email,
            grade: studentData.grade,
            course: studentData.course
          })
        });
        if (!response.ok) throw new Error("Failed to update student");
        this.editingStudent = null;
        await this.loadStudents();
      } catch (error) {
        console.error(error);
      }
    },

    // TODO: handleDelete(id)
    // - DELETE to API_URL + "/students/" + id + "/"
    // - Remove from this.students
    async handleDelete(id) {
      try {
        const response = await this.authFetch(`${API_URL}/students/${id}/`, {
          method: "DELETE"
        });
        if (!response.ok) throw new Error("Failed to delete student");
        this.students = this.students.filter(student => student.id !== id);
      } catch (error) {
        console.error(error);
      }
    },

    handleFiltersChanged({ grade, courseId }) {
      this.filterGrade = grade;
      this.filterCourse = courseId;
    },

    setTokens(accessToken, refreshToken) {
      if (accessToken) localStorage.setItem("access_token", accessToken);
      if (refreshToken) localStorage.setItem("refresh_token", refreshToken);
    },

    getToken() {
      return localStorage.getItem("access_token")
    },

    getRefreshToken() {
      return localStorage.getItem("refresh_token")
    },

    async refreshAccessToken() {
      const refreshToken = this.getRefreshToken();
      if (!refreshToken) return false;

      try {
        const response = await fetch(`${API_URL}/token/refresh/`, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ refresh: refreshToken })
        });

        if (!response.ok) return false;

        const data = await response.json();
        this.setTokens(data.access, data.refresh);
        return true;
      } catch (error) {
        console.error(error);
        return false;
      }
    },

    async authFetch(url, options = {}, retryOnUnauthorized = true) {
      const headers = {
        ...(options.headers || {}),
        Authorization: `Bearer ${this.getToken()}`
      };

      const response = await fetch(url, {
        ...options,
        headers
      });

      if (response.status !== 401 || !retryOnUnauthorized) {
        return response;
      }

      const refreshed = await this.refreshAccessToken();
      if (!refreshed) {
        this.handleLogout();
        throw new Error("Session expired. Please log in again.");
      }

      const retryHeaders = {
        ...(options.headers || {}),
        Authorization: `Bearer ${this.getToken()}`
      };

      return fetch(url, {
        ...options,
        headers: retryHeaders
      });
    }
  },

  mounted() {
    // TODO: check if token exists in localStorage
    // If yes, set isLoggedIn = true and call this.loadStudents()
    const token = this.getToken();
    if (token) {
      this.isLoggedIn = true;
      this.loadCourses();
      this.loadStudents();
    }
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

.error {
  margin: 12px 0;
  color: #b91c1c;
  font-weight: 600;
}

h2 {
  margin-bottom: 20px;
  color: #0f172a;
}
</style>
