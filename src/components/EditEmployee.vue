<template>
  <div id="edit-employee">
      <h3>Edit Employee</h3>
      <div class="row">
        <form @submit.prevent="updateEmployee" class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="employee_id" required disabled>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="name" required>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="dept" required>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="position" required>
            </div>
          </div>
          <button type="submit" class="btn">Submit</button>
          <router-link to="/" class="btn grey">Cancel</router-link>
        </form>
      </div>
  </div>
</template>

<script>
  //import database
  import db from './firebaseInit'
  export default{
    name: 'edit-employee',
    data(){
      return{
        employee_id: null,
        name: null,
        dept: null,
        position: null
      }
    },
    beforeRouteEnter(to, from, next){
      //get db employees collection
      db.collection('employees')
      //match employee id with url id
      .where('employee_id', '==', to.params.employee_id).get()
      //if success
      .then(querySnapshot => {
        //loop through all data in db
        querySnapshot.forEach(doc =>{
          next(vm => {
            //load this
            vm.employee_id = doc.data().employee_id
            vm.name = doc.data().name
            vm.dept = doc.data().dept
            vm.position = doc.data().position
          })
        })
      })
    },
    //if route enters do fetchData
    watch:{
      '$route': 'fetchData'
    },
    //declare fetchData
    methods: {
      fetchData(){
        db.collection('employees')
        //if employee_id equals routeURL employee_id
        .where('employee_id', '==', this.$route.params.employee_id)
        //fetch this
        .then(querySnapshot => {
          //for each item
          querySnapshot.forEach(doc => {
            this.employee_id = doc.data().employee_id
            this.name = doc.data().name
            this.dept = doc.data().dept
            this.position = doc.data().position
            })
          })
        },
        updateEmployee(){
          db.collection('employees')
          //if employee_id equals routeURL employee_id
          .where('employee_id', '==', this.$route.params.employee_id).get()
          //fetch this
          .then(querySnapshot => {
            //for each item
            querySnapshot.forEach(doc => {
              doc.ref.update({
                employee_id: this.employee_id,
                name: this.name,
                dept: this.dept,
                position: this.position
              })
              .then(() => {
                this.$router.push({name: 'view-employee', params: {employee_id: this.employee_id}})
              })
            })
          })
        }
      }
    }
</script>
