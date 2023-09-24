<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4> Companies list
          <RouterLink to="/company/create" class="btn btn-primary float-end">Add Company </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <caption>List of companies</caption>
          <thead style="text-align: center;" class="table-dark">
            <tr>
              <th>Id</th>
              <th >Logo</th>
              <th>Name</th>
              <th>Email</th>
              <th>Website</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody v-if="this.companies.length > 0" style="text-align: justify;" class="table-info">
            <tr v-for="(company, index) in this.companies" :key="index">
              <td>{{ company.id }}</td>
              <td>
                <img :src="company.logo" alt="Company Logo">
              </td>
              <td>{{ company.name }}</td>
              <td>{{ company.email }}</td>
              <td>{{ company.website }}</td>
              <td style="text-align: center">
                <RouterLink :to="{ path: '/company/'+'edit/' }" class="btn btn-info">Edit</RouterLink>
                <span style="margin-left: 10px; margin-right: 10px;"></span>
                <button type="button" @click="deleteCompany()" class="btn btn-danger">Delete</button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr class="table-info">
              <td colspan="6">Loading...</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'companies',
  data(){
    return{
      companies:[]
    }
  },
  mounted() {
    this.getCompanies();
  },

  methods: {
    getCompanies() {
      axios.get("http://localhost:8000/api/companies")
          .then(res => {
            this.companies = res.data;
            // console.log(res.data);
            console.log('Companies data:', this.companies);

          })
          .catch(error=> {
            console.log('error fetching companies',error)
          });
    }
  }
}

</script>