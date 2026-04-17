<template>
        <div class="filter-wrap">
        <button class="btn-toggle" @click="showFilters = !showFilters" type="button">Toggle Filters</button>
        <form v-if="showFilters" @submit.prevent="submit">
      <select v-model="grade">
        <option value="">All Grades</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
        <option>D</option>
        <option>F</option>
      </select>
      <select id="student-course" v-model="courseId">
        <option value="">All Courses</option>
        <option v-for="course in courses" :key="course.id" :value="course.id">
          {{ course.code }} - {{ course.name }}
        </option>
      </select>
                <button class="btn-apply" type="submit">Apply Filters</button>
                <button class="btn-clear" @click="clearFilters" type="button">Clear Filters</button>
    </form>
        </div>
</template>
<script>
export default {
    emits : ['filters-changed'],    
    props: {
        courses: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            showFilters: false,
            name: "",
            email: "",
            grade: "",
            courseId: ""

        };
    },
    methods: {
        submit() {
            this.$emit("filters-changed", {
                name: this.name,
                email: this.email,
                grade: this.grade,
                courseId: this.courseId
            });
        },
        clearFilters() {
            this.name = "";
            this.email = "";
            this.grade = "";
            this.courseId = "";
            this.$emit("filters-changed", {
                name: this.name,
                email: this.email,
                grade: this.grade,
                courseId: this.courseId
            });
        }
    }
};
</script>

<style scoped>
.filter-wrap {
    margin-bottom: 14px;
}

form {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 10px;
    margin-top: 10px;
    padding: 12px;
    background: #f8fafc;
    border-radius: 10px;
    border: 1px solid #e2e8f0;
}

select {
    width: 100%;
    padding: 10px 12px;
    border: 1px solid #cbd5e1;
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

.btn-toggle {
    background: #0f172a;
    color: #f8fafc;
    box-shadow: 0 6px 16px rgba(15, 23, 42, 0.22);
    /* Add some margin to separate from the form */
    margin-bottom: 10px;
    margin-top: 10px;
}

.btn-toggle:hover {
    background: #1e293b;
}

.btn-apply {
    background: #2d59b9;
    color: #fffbeb;
    box-shadow: 0 6px 16px rgba(45, 89, 185, 0.24);
}

.btn-apply:hover {
    background: #133a8f;
}

.btn-clear {
    background: #9ca3af;
    color: #111827;
}

.btn-clear:hover {
    background: #6b7280;
    color: #ffffff;
}
</style>