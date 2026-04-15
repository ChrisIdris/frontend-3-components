<template>
  <!-- TODO: build the nav bar -->
  <!-- Show the app title -->
  <!-- When logged in: show username and a logout button -->
  <!-- The logout button should emit 'logout' when clicked -->
  <header>
    <nav>
      <h1>Student Dashboard</h1>
      <!-- TODO: logged-in section here -->
    </nav>
  </header>
</template>

<script>
export default {
  props: {
    // TODO: define props for username (String) and isLoggedIn (Boolean)
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

/* TODO: add styles for the username display and logout button */
</style>
