<template>
	<div class="card">
		<h3>Edit Student</h3>
		<form @submit.prevent="submit">
			<input v-model="name" type="text" placeholder="Name" required />
			<input v-model="email" type="email" placeholder="Email" required />
			<select v-model="grade" required>
				<option value="" disabled>Select Grade</option>
				<option>A</option>
				<option>B</option>
				<option>C</option>
				<option>D</option>
				<option>F</option>
			</select>
			<select v-model.number="courseId" required>
				<option value="" disabled>Select a course</option>
				<option v-for="course in courses" :key="course.id" :value="course.id">
					{{ course.code }} - {{ course.name }}
				</option>
			</select>
			<div class="actions">
				<button type="submit" class="btn-save">Save Changes</button>
				<button type="button" class="btn-cancel" @click="$emit('cancel')">Cancel</button>
			</div>
		</form>
	</div>
</template>

<script>
export default {
	props: {
		student: {
			type: Object,
			required: true
		},
		courses: {
			type: Array,
			required: true
		}
	},

	emits: ['student-updated', 'cancel'],

	data() {
		return {
			name: '',
			email: '',
			grade: '',
			courseId: ''
		}
	},

	watch: {
		student: {
			immediate: true,
			handler() {
				this.syncForm();
			}
		}
	},

	methods: {
		syncForm() {
			this.name = this.student.name || '';
			this.email = this.student.email || '';
			this.grade = this.student.grade || 'A';
			this.courseId = this.student.course || '';
		},

		submit() {
			this.$emit('student-updated', {
				id: this.student.id,
				name: this.name,
				email: this.email,
				grade: this.grade,
				course: this.courseId
			});
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

input,
select {
	display: block;
	width: 100%;
	padding: 10px 12px;
	margin-bottom: 12px;
	border: 1px solid #ccc;
	border-radius: 8px;
}

.actions {
	display: flex;
	gap: 12px;
}

.actions button {
	width: 100%;
	border: none;
	border-radius: 10px;
	padding: 10px 14px;
	font-weight: 600;
	cursor: pointer;
	transition: transform 0.15s ease, box-shadow 0.2s ease, background-color 0.2s ease;
}

.actions button:hover {
	transform: translateY(-1px);
}

.actions button:active {
	transform: translateY(0);
}

.btn-save {
	background: #0369a1;
	color: #f0f9ff;
	box-shadow: 0 6px 16px rgba(3, 105, 161, 0.28);
}

.btn-save:hover {
	background: #075985;
}

.btn-cancel {
	background: #475569;
	color: #f8fafc;
}

.btn-cancel:hover {
	background: #334155;
}
</style>
