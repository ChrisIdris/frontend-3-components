<template>
  <!-- TODO: an input with v-model that emits the search term when it changes -->
  <!-- Hint: use @input="$emit('search', searchTerm)" -->
  <div class="search-bar">
    <input 
      type="text" 
      v-model="searchTerm" 
      @input="$emit('search', searchTerm)" 
      placeholder="Search by name, email, or course..."
    />
  </div>
</template>

<script>
export default {
  emits: ['search'],

  data() {
    return {
      // TODO: local searchTerm
      searchTerm: ""
    }
  }
}
</script>

<style scoped>
/* TODO: style the search input with flex */
.search-bar {
  display: flex;
  justify-content: center;
  align-items: center;
}
.search-bar input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
