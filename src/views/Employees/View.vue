<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4> Employee list
          <RouterLink to="/employee/create" class="btn btn-primary float-end">Add Employee </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <caption>List of employees</caption>
          <thead style="text-align: center;">
            <tr>
              <th>id</th>
              <th >Name</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Company</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody style="text-align: justify;" v-if="this.employees.length > 0">
            <tr v-for="(employee,index) in this.employees" :key="index">
              <td>{{ employee.id }}</td>
              <td>{{ employee.name}}</td>
              <td>{{ employee.email }}</td>
              <td>{{ employee.phone }}</td>
              <td>{{ employee.company_name}}</td>
              <td>
                <RouterLink :to="{ path: '/employee/'+'edit/'+employee.id }" class="btn btn-info">Edit</RouterLink>
                <span style="margin-left: 10px; margin-right: 10px;"></span>
                <button type="button" @click="deleteEmployee(employee.id)" class="btn btn-danger">Delete</button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr>
              <td colspan="5">Loading...</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: 'employees',
  data(){
    return {
      employees:[]
    }
  },
  mounted() {

    this.getEmployees();
  },
  methods: {
    getEmployees(){
      axios.get("http://localhost:8000/api/employees")
          .then(res => {
            this.employees = res.data.data;
            // console.log(this.employees)
          })
          .catch(error=>{
            console.error('error fetching employees', error);
          });
    },
    deleteEmployee(id) {
      if(confirm("Are you sure, you want to delete?")) {
        // console.log(id);
        axios.delete(`http://localhost:8000/api/employees/${id}`)
            .then(res => {
              alert(res.data.message);
              this.getEmployees();
            })
      }
    }
  },
}
</script>