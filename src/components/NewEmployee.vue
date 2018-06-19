<template>
  <div id="new-employee">
      <h3>New Employee</h3>
      <div class="row">
        <form @submit.prevent="saveEmployee" class="col s12">
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="employee_id" required>
              <label>Employee ID#</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="name" required>
              <label>Name</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="dept" required>
              <label>Department</label>
            </div>
          </div>
          <div class="row">
            <div class="input-field col s12">
              <input type="text" v-model="position" required>
              <label>Position</label>
            </div>
          </div>
          <button type="submit" class="btn">Submit</button>
          <router-link to="/" class="btn grey">Cancel</router-link>
        </form>
      </div>
  </div>
</template>

<script>
  //importing db
  import db from './firebaseInit'
  export default{
    name: 'new-employee',
    data(){
      //return empty
      return{
        employee_id: null,
        name: null,
        dept: null,
        position: null
      }
    },
    methods:{
      saveEmployee(){
        //get db collection
        db.collection('employees').add({
          //add this to collection
          //this is bound to v-bind/v-model
          employee_id: this.employee_id,
          name: this.name,
          dept: this.dept,
          position: this.position
        })
        //after promise of add
        .then(docRef =>  this.$router.push('/'))
        //if something goes wrong
        .catch(error => console.log(error))
      }
    }
  }
</script>
