<template>
  <!-- TODO: build the nav bar -->
  <!-- Show the app title -->
  <!-- When logged in: show username and a logout button -->
  <!-- The logout button should emit 'logout' when clicked -->
  <header>
    <nav>
      <h1>Student Dashboard</h1>
      <!-- TODO: logged-in section here -->
      <div v-if="isLoggedIn">
        
        <span class="username">{{ username }}</span>
        <button class="btn-logout" @click="$emit('logout')">Logout</button>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  props: {
    username: String,
    isLoggedIn: Boolean
  },

  emits: ['logout']
}
</script>

<style scoped>
header {
  background-color: #1e293b;
}

nav {
  max-width: 900px;
  margin: 0 auto;
  padding: 15px 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

nav h1 {
  color: #e2e8f0;
  font-size: 20px;
}

.username {
  color: #e2e8f0;
  margin-right: 15px;
}

.btn-logout {
  background-color: #ef4444;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: transform 0.15s ease, background-color 0.2s ease;
}

.btn-logout:hover {
  background-color: #dc2626;
  transform: translateY(-1px);
}

.btn-logout:active {
  transform: translateY(0);
}
</style>
