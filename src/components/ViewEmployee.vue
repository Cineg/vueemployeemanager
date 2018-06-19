<template>
  <div id="view-employee">
      <ul class="collection with-header">
        <li class="collection-header">
          <h4>{{name}}</h4>
        </li>
        <li class='collection-item'>
          Employee ID#: {{employee_id}}
        </li>
        <li class='collection-item'>
          Department: {{dept}}
        </li>
        <li class='collection-item'>
          Position: {{position}}
        </li>
      </ul>
      <router-link to="/" class="btn grey"> Back</router-link>
      <button @click="deleteEmployee" class="btn red">Delete</button>

      <div class="fixed-action-btn">
        <router-link v-bind:to="{name: 'edit-employee', params:{employee_id: employee_id}}" class="btn-floating btn-large blue">
          <i class="fas fa-pencil-alt"></i>
        </router-link>
      </div>
  </div>
</template>

<script>
  //import db
  import db from './firebaseInit'
  export default{
    name: 'view-employee',
    data(){
      return{
        employee_id: null,
        name: null,
        dept: null,
        position: null
      }
    },
    //before view is rendered
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
      deleteEmployee() {
        if(confirm('Are you sure?')){
          db.collection('employees')
          //if employee_id equals routeURL employee_id
          .where('employee_id', '==', this.$route.params.employee_id).get()
          //fetch this
          .then(querySnapshot => {
            //for each item
            querySnapshot.forEach(doc => {
              doc.ref.delete()
              this.$router.push('/')
            })
          })
        }
      }
    }
  }
</script>
