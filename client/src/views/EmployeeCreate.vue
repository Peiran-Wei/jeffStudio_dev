<template>
  <b-list-group>
      <div id="textColorEm">
        <h1> THIS IS THE EMPLOYEE CREATE PAGE</h1>
        <label for="fname">First Name:</label>
        <input type="text" id="fname" name="fname"><br><br>
        <label for="lname">Last Name:</label>
        <input type="text" id="lname" name="lname"><br><br>
        <label for="userName">User Name:</label>
        <input type="text" id="userName" name="userName"><br><br>
        <label for="userPass">Password:</label>
        <input type="text" id="userPass" name="userPass"><br><br>
        <label for="optionbox">Company</label>
        <select class="groupContainer" id=optionbox >
            <option v-for="(company, id) in companies" :key="id"
                v-bind:value="company" class="groupValue">
                {{ company.name }}
            </option>
        </select>
        <button class="backBut" @click="cancel()">Cancel</button>
        <button class="confirmBut" @click="saveEmployee()">saveEmployee</button>
      </div>
      <myFooter1/>
  </b-list-group>
</template>

<script>
import { Api } from '@/Api'
import myFooter1 from '@/components/createFooter.vue'

export default {
  name: 'employeeCreate',
  props: ['companyObj'],
  components: {
    myFooter1
  },
  data() {
    return {
      birdy: {
        fname: '',
        lname: '',
        userName: '',
        userPass: '',
        companys: []
      },
      companies: []
    }
  },
  created() {
    this.getCompanies()
  },
  methods: {
    saveEmployee() {
      this.birdy.fname = document.getElementById('fname').value
      this.birdy.lname = document.getElementById('lname').value
      this.birdy.userName = document.getElementById('userName').value
      this.birdy.userPass = document.getElementById('userPass').value
      this.birdy.companys = this.companies[document.getElementById('optionbox').selectedIndex]
      Api.post('/companies/' + this.companies[document.getElementById('optionbox').selectedIndex]._id + '/employees', this.birdy)
        .then(response => {
          this.$router.push({ name: 'employeeView', params: { companyObject: this.companyObj } })
        })
        .catch(error => {
          console.log(error)
        })
    },
    cancel() {
      this.$router.push({ name: 'employeeView', params: { companyObject: this.companyObj } })
    },
    getCompanies() {
      this.companies = []
      Api.get('/companies').then(response => {
        this.companies = []
        this.companies = response.data.companies
      })
    }
  }
}
</script>
<style scoped src="@/components/CSS_components.css">
</style>
