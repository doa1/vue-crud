<template>
  <div id="app" class="small-container">
    <h1>Simple Vue Tutorial App</h1>
    <test-component />
    <EmpoyeeForm @add:employee="addEmployee"/>
    <employee-table v-bind:employees="employees" @edit:employee="editEmployee" @delete:employee="deleteEmployee"/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import EmployeeTable from '@/components/EmployeeTable'
import EmpoyeeForm from '@/components/EmployeeForm'
import TestComponent from './components/TestComponent'
const BASE_URL="https://jsonplaceholder.typicode.com/users"
const headers = { 'Content-type': 'application/json; charset=UTF-8' }

export default {
  name: 'App',
  components: {
    //HelloWorld
    EmployeeTable,
    EmpoyeeForm,
    TestComponent
  },
  data (){
    return {
      employees:[
        /*{
          id: 1,
          name: 'Richard Hendricks',
          email: 'richard@piedpiper.com',
        },
        {
          id: 2,
          name: 'Bertram Gilfoyle',
          email: 'gilfoyle@piedpiper.com',
        },
        {
          id: 3,
          name: 'Dinesh Chugtai',
          email: 'dinesh@piedpiper.com',
        },*/
      ]
    }
  },
  mounted(){
    return this.getEmployees()
  },
  methods:{
    getEmployees: async function(){
      try {
        const response = await fetch(BASE_URL);
        const data = await response.json();
        this.employees =data;
        console.log(data);
        
      } catch (error) {
        console.log(error)
      }
      
    },

    addEmployee: async function(employee){
      //grab and add this to the array
      console.log(employee);
      //in real world just do this since id is auto field usually
      //this.employees=[...this.employees,employee]
      //get the last id and increment

      /*const lastId = this.employees.length > 0 ? this.employees[this.employees.length - 1].id
                    : 0;
      const id =lastId+1;
      var newEmployee = {...employee,id};
      this.employees=[...this.employees,newEmployee];*/

      try {
        const response = await fetch(BASE_URL,{
          headers:headers,
          method:'POST',
          body:JSON.stringify(employee)
        });
        console.log(response)
        const data = await response.json();
        this.employees=[...this.employees,data]
        console.log(data)
      } catch (error) {
        console.log(error)
      }

    },
    editEmployee: async function(id,employee){
      //let employee=this.employees[id-1];
      console.log(employee);
      try {
        const response = await fetch(`${BASE_URL}/${id}`,{
          method:'PUT',
          headers:headers,
          body:JSON.stringify(employee)
        });
        const data = await response.json();
        this.employees =this.employees.map(employee=>(employee.id ==id?data:employee))

      } catch (error) {
        console.log(error);
      }
    },
    deleteEmployee:async function(id){
      //this.employees.pop(id-0);
      try {
        const response = await fetch(`${BASE_URL}/${id}`,{
          headers: headers,
          method: 'DELETE',
         // body:JSON.stringify()
        })
        console.log(response);
        this.employees= this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.log(error)
      }
      
    }

  }
}
</script>

<style>
button {
    background: #009435;
    border: 1px solid #009435;
  }
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.small-container{
  min-width: 780px;
}
</style>
