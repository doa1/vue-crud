<template>
    <div id="employee-form">
        <p v-if="error" style="color:red">All Fields Are Required</p>
        <form @submit.prevent="handleSubmit">
            <label>Employee name {{employee.name}}</label>
            <input v-model="employee.name" ref="first" type="text" @focus="clearStatus" :class="{'has-error': error && invalidName }" />
            <label>Employee Email {{employee.email}}</label>
            <input v-model="employee.email" type="text" @focus="clearStatus" :class="{'has-error': error && invalidEmail }" />
            <button>Add Employee</button>
        </form>
        <p v-if="success" class="success-message">Successfully added the user</p>
    </div>
</template>
<script>
export default {
    name:'employee-form',
    data(){
       return{
        //set states
        success:false,
        error:false,
        submitting:false,
        message:'',
        //model for the form entries

        employee:{ 
            name:'',
            email:''
         }
        }
    },
    methods:{
        //emitter
       handleSubmit(){
           //var name=this.employee.name;
           //var email =this.employee.email;
           
           //Emitting events to the parent
           //this.$emit('name-of-emitted-event', dataToPass)
          this.submitting=true;
          this.clearStatus();
          if (this.invalidName || this.invalidEmail) {
              this.error=true;
              //stop exec
              return;
          }

          
          this.$emit('add:employee',this.employee);
          this.employee.name="";
          this.employee.email="";
          this.$refs.first.focus();

          this.error=false;
          this.success=true;
          this.submitting=false;

          //Retrieving events from the child
          /**
           * Now employee-form is broadcasting its emitted event, but we need to capture the event and value in the parent to work with it.

            *The first thing we need to do is make employee-form acknowledge and handle the emitted event, and invoke a new method. It will look like this:

                <component @name-of-emitted-event="methodToCallOnceEmitted"></component>
           */
       },
       clearStatus(){
           //will clear the form fields and reset status after submit
           this.error=false;
           this.success=false;
           this.submitting=false;
           this.message='';
       }
       
    },
    //computed properties functions that are automatically computed when something changes.
    computed:{
        invalidName(){
            return this.employee.name ==='';
        },
        invalidEmail(){
            return this.employee.email ==='';
        }
    }

}
</script>
<style scoped>
    form {
    margin-bottom: 2rem;
    padding-left: 90px;
    padding-right: 90px
  }
  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>