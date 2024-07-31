<template>
  <div class="mt-5 container">
    <div class="card">
      <div class="card-header">
        <h4>Student List
          <NuxtLink to="/students/create" class="btn btn-primary float-end">Add Student</NuxtLink>
        </h4>
      </div>
      <div class="card-body">
        <div v-if="isLoading">
          <Loading title="Loading..."></Loading>
        </div>
        <div v-else>
          <table class="table table-striped table-bordered">
            <tr>
              <td>ID</td>
              <td>Name</td>
              <td>Course</td>
              <td>Email</td>
              <td>Phone</td>
              <td>Action</td>
            </tr>
            <tr v-for="(row, index) in listData" :key="index">
              <td>{{ row.id }}</td>
              <td>{{ row.name }}</td>
              <td>{{ row.course }}</td>
              <td>{{ row.email }}</td>
              <td>{{ row.phone }}</td>
              <td>
                <NuxtLink :to="`/students/${row.id}`" class="btn btn-success btn-sm mx-2">Edit</NuxtLink>
                <button type="button" @click="actionDelete($event, row.id)" class="btn btn-danger btn-sm mx-2">Delete</button>
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'students',
  data() {
    return {
      listData: {},
      isLoading: false,
    }
  },
  mounted() {
    this.getData()
  },
  methods: {
    getData() {
      this.isLoading = true;
      let self = this;
      axios.get('http://localhost:8000/api/students').then(res => {
        self.isLoading = false
        self.listData = res.data.students;
      }).catch(error => {
        console.log(error);
        self.isLoading = false
      })
    },
    actionDelete(event, id) {
      if (confirm('are u sure, you want to delete?')) {
        event.target.innerText = 'Delete on proses'
        axios.delete('http://localhost:8000/api/students', id).then(res => {
          event.target.innerText = 'Deleted'
          this.getData()
        }).catch(error => {
          console.log(error);
        })
      }
    }
  },
}
</script>

<style></style>