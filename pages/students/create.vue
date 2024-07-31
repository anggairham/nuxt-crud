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
              <button type="submit" class="btn btn-primary">Save</button>
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
      student: {
        name: '',
        course: '',
        email: '',
        phone: '',
      },
      isLoading: false,
      isLoadingText: 'Loading',
      errorList: {}
    }
  },
  methods: {
    saveForm() {
      this.isLoading = true;
      this.isLoadingText = 'Saving';

      let self = this

      axios.post('http://localhost:8000/api/students', this.student).then(res => {
        console.log(res);
        self.student.name = ''
        self.student.course = ''
        self.student.email = ''
        self.student.phone = ''

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