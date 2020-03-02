<template>
    <div id="test-component">
        <p>Sum {{sum}}</p>
        
    </div>
</template>
<script>
export default {
    data(){
        return {
            a:5,
            b:6,
            
            samples:{
                name:'Test'
            }
        }
    },
    methods:{
        add:function(){
            return this.a+this.b;
        }
    },
    computed:{
        sum(){
            return this.a+this.b;
        }
    }
}
</script>