<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Add Employees</h4>
      </div>
      <div class="card-body">
        <ul class="alert alert-danger" v-if="Object.keys(this.errorList).length > 0" >
          <li v-for="(error, index) in this.errorList" :key="index">
            {{ error[0] }}
          </li>
        </ul>
        <div class="mb-3">
          <label for="name">Name</label>
          <input type="text" id="name" v-model="model.employee.name" class="form-control" autocomplete="on">
        </div>
        <div class="mb-3">
          <label for="email">Email</label>
          <input type="text" id="email" v-model="model.employee.email" class="form-control" autocomplete="on">
        </div>
        <div class="mb-3">
          <label for="phone">Phone Number</label>
          <input type="text" id="phone" v-model="model.employee.phone" class="form-control" autocomplete="on">
        </div>
        <div class="mb-3">
          <div>Select Company</div>
          <select class="form-select" id="company_id" v-model="model.employee.company_id" autocomplete="on">
            <option value="">Select a company</option>
            <option v-for="company in companies" :key="company.id" :value="company.id">{{ company.name }}</option>
          </select>
        </div>
        <div class="mb-3 mt-5">
          <router-link to="/employee" class="btn btn-danger me-2">Cancel</router-link>
          <button type="button" @click="createEmployee" class="btn btn-primary">Create</button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios"

export default {
  name: 'employeeCreate',
  data(){
    return {
      errorList: {},
      companies:[],
      model: {
        employee: {
          name: '',
          email: '',
          phone: '',
          company_id: ''
        }
      }

    }
  },
  mounted() {

    this.getCompanies();
  },
  methods: {
    getCompanies(){
      axios.get("http://localhost:8000/api/companies/")
          .then(res => {
            this.companies = res.data;
            console.log(this.companies);
          })
          .catch(error=>{
            console.error('error fetching companies', error);
          });
    },
    createEmployee(){

      axios.post("http://localhost:8000/api/employees", this.model.employee)
          .then(res => {
            console.log(res)
            alert(res.data.message);
            this.model.employee = {
              name: '',
              email: '',
              phone: '',
              company_id: ''
            }
            this.errorList = '';
            this.$router.push('/employee');
          })
          .catch(error => {
            if(error.response && error.response.status === 422){
              this.errorList = error.response.data.errors;
            } else if (error.request) {
              console.log(error.request);
            } else {
              console.log('Error', error.message);
            }
          })
    }
  },
}
</script>