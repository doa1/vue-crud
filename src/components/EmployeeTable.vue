<template>
    <div id="employee-table">
        <p v-if="employees.length < 1" class="empty-table">No Employees at the moment</p>
        <table v-else class="table table-responsive table-bordered">
            <thead>
                <th>Employee Name</th>
                <th>Employee Email</th>
                <th>Action </th>
            </thead>
            <tbody>
        <tr v-for="employee in employees" :key="employee.id">
          <td v-if="editing === employee.id">
              <input v-model="employee.name" type="text" :class="{'has-error':error}">
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="editing === employee.id">
              <input type="email" v-model="employee.email">
          </td>
          <td v-else>{{employee.email}}</td>
          <td v-if="editing === employee.id">
              <button class="btn btn-success" @click="editEmployee(employee)">Save</button>
              <button class="muted-button" @click="editing =null">Cancel</button>
          </td>
          <td v-else>
              <button class="btn btn-success" @click="editMode(employee.id)">Edit</button>
              
              <button class="btn btn-warning" @click="$emit('delete:employee',employee.id)"> Delete</button>
          </td>
        </tr>
        
      </tbody>
        </table>
    </div>
</template>
<script>
export default {
    name:'employee-table',
    //tell it to receive the employee arrays as props
    props:{
        employees:Array
    },
    data(){
        return{
            editing:null,
            error:false,
            success:false

        }
    },
    methods:{
        editMode:function(id){
            this.editing =id;
        },
        disableEditing:function(){
            this.editing=null;
        },
        editEmployee:function(employee){
           if (employee.name ==='' || employee.email ==='' ) {
               this.error=true;
               
               return;
           }
           this.$emit('edit:employee',employee.id,employee);
           this.success=true;
           this.editing=null;
          
           
        },
        deleteEmployee:function(id){
            //let employee=this.employees[id-1];
            this.employees.pop(id-1);
        }
    }
   
}
</script>

<style scoped>
    #employee-table{
        flex: 3;
        margin-left: 20px
    }
    button{
        margin: 5px
    }
    .empty-table{
        color: red;
        font-size: 18px;
        font-weight: 900
    }
</style>