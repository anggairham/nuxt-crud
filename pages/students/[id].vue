<template>
  <div class="mt-5 container">
    <div class="card">
      <div class="card-header">
        <h4>Add Student</h4>
        <NuxtLink to="/students" class="btn btn-danger float-end">Back</NuxtLink>
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading :title="isLoadingText"></Loading>
        </div>
        <div v-else>
          <form @submit.prevent="saveForm">
            <div class="mb-3">
              <label for="">Name</label>
              <input v-model="student.name" type="text" class="form-control">
              <span class="text-danger" v-if="errorList.name">{{ errorList.name[0] }}</span>
            </div>
            <div class="mb-3">
              <label for="">Course</label>
              <input v-model="student.course" type="text" class="form-control">
              <span class="text-danger" v-if="errorList.course">{{ errorList.course[0] }}</span>
            </div>
            <div class="mb-3">
              <label for="">Email</label>
              <input v-model="student.email" type="text" class="form-control">
              <span class="text-danger" v-if="errorList.email">{{ errorList.email[0] }}</span>
            </div>
            <div class="mb-3">
              <label for="">Phone</label>
              <input v-model="student.phone" type="text" class="form-control">
              <span class="text-danger" v-if="errorList.phone">{{ errorList.phone[0] }}</span>
            </div>
            <div class="mb-3">
              <button type="submit" class="btn btn-primary">Update</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "studentCreate",
  data() {
    return {
      studentId: '',
      student: {
        name: '',
        course: '',
        email: '',
        phone: '',
      },
      isLoading: false,
      isLoadingText: 'Loading',
      errorList: {},
    }
  },
  mounted() {
    this.studentId = this.$route.params.id
    this.getStudent()
  },
  methods: {
    getStudent(studentId) {
      let self = this

      axios.get(`http://localhost:8000/api/students/${studentId}`).then(res => {
        self.student = res.data.student
      }).catch(function (error) {
        console.log(error);
      })
    },
    saveForm() {
      this.isLoading = true;
      this.isLoadingText = 'Updating';

      let self = this

      axios.put('http://localhost:8000/api/students', this.student).then(res => {
        // self.errorList = {}
        self.isLoading = false;
        self.isLoadingText = 'Loading';
      }).catch(function (error) {
        console.log(error);
        if (error.response.status == 422) {
          self.errorList = error.response.data.errors
        }
        self.isLoading = false;
        self.isLoadingText = 'Loading';
      })
    }
  },
}
</script>

<style></style>