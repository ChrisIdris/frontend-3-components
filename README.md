# Student Dashboard — Vue Components

## Overview

Rebuild the student dashboard as a **component-based Vue app** using Vite. Break the single-file CDN version into separate .vue components with props, events, and scoped styles.

---

## Setup

```bash
cd student-dashboard
npm install
npm run dev
```

Open http://localhost:5173. Your Django backend must be running at localhost:8000.

---

## Files Provided

All component files are created with TODOs inside. The project structure is ready — you just need to fill in the logic.

```
src/
  main.js                    ← mounts the app (done, don't touch)
  App.vue                    ← root component — fill in TODOs
  components/
    NavBar.vue               ← fill in TODOs
    LoginForm.vue            ← fill in TODOs
    SearchBar.vue            ← fill in TODOs
    AddStudentForm.vue       ← fill in TODOs
    StudentCard.vue          ← fill in TODOs
```

---

## How It All Connects

```
App.vue (holds data + API logic)
  │
  ├── NavBar           :username, :is-logged-in  →  @logout
  │
  ├── LoginForm                                  →  @login { username, password }
  │
  ├── SearchBar                                  →  @search "term"
  │
  ├── AddStudentForm                             →  @student-added { name, email, grade, course }
  │
  └── StudentCard      :student                  →  @delete studentId
      (one per student, rendered with v-for)
```

**Props go down (:)** — parent passes data to children.
**Events go up (@)** — children tell the parent something happened.

---

## Step by Step

### 1. Start with NavBar
- Define props: `username` (String), `isLoggedIn` (Boolean)
- Show the title always
- When logged in, show the username and a logout button
- The logout button emits 'logout': `@click="$emit('logout')"`
- In App.vue: import, register, and use `<NavBar :username="username" :is-logged-in="isLoggedIn" @logout="handleLogout" />`

### 2. LoginForm
- Local data: username, password, loginError
- Form with @submit.prevent calling a submit method
- submit() emits 'login' with `{ username: this.username, password: this.password }`
- In App.vue: show with `v-if="!isLoggedIn"`, listen with `@login="handleLogin"`
- App.vue's handleLogin does the actual fetch to /api/token/

### 3. StudentCard
- Props: student (Object, required)
- Display: name, email, grade
- Delete button emits 'delete' with student.id
- In App.vue: use with `v-for` and listen with `@delete="handleDelete"`

### 4. AddStudentForm
- Local data: name, email, grade, courseId
- Form with v-model on each field
- submit() emits 'student-added' with the form data, then clears the fields
- App.vue's handleStudentAdded does the fetch POST

### 5. SearchBar
- Local data: searchTerm
- Input with v-model, emits 'search' on @input
- App.vue listens with `@search="searchTerm = $event"` and uses searchTerm in a computed

### 6. App.vue
- Import and register all components
- Wire up template with v-if for login/dashboard, v-for for student cards
- Implement all methods: handleLogin, handleLogout, loadStudents, handleStudentAdded, handleDelete
- Computed: filteredStudents filters by searchTerm
- mounted(): check for existing token

---

## Checklist

- [ ] NavBar shows title, username when logged in, logout works
- [ ] LoginForm submits credentials, App handles the API call
- [ ] StudentCard displays student data, delete button works
- [ ] AddStudentForm submits new student data, list refreshes
- [ ] SearchBar filters the student list
- [ ] v-if toggles between login and dashboard
- [ ] All components use `<style scoped>`
- [ ] Data lives in App.vue, children use props and events

---

## Bonus Challenges

- [ ] **CourseList.vue** — fetch and display courses from /api/courses/
- [ ] **Edit mode** — StudentCard toggles between view and inline edit form
- [ ] **LoadingSpinner.vue** — reusable loading indicator
- [ ] **ErrorMessage.vue** — reusable error display

---

## When You're Done

```bash
git add .
git commit -m "Session 10: Vue component-based student dashboard"
git push
```

**Next session**: Vue Router and state management with Pinia.
