<template>
    <Header />
    <h1>Add Employee Page</h1>
    <form class="add">
        <input type="text" name="fname" placeholder="First Name" v-model="employee.fname">
        <input type="text" name="lname" placeholder="Last Name" v-model="employee.lname">
        <input type="text" name="mname" placeholder="Middle Name" v-model="employee.mname">
        <button type="button" v-on:click="addEmployee">Add New Employee</button>
    </form>
</template>

<script>
import Header from './Header.vue'
import axios from 'axios'
export default {
    name: 'Add',
    components:{
        Header
    },
    data(){
        return{
            employee:{
                fname: '',
                lname: '',
                mname: ''
            }
        }
    },
    methods:{
        async addEmployee(){
            const result = await axios.post("http://localhost:3000/employee", 
            {
                fname: this.employee.fname,
                lname: this.employee.lname,
                mname: this.employee.mname
            })
            if(result.status == 201){
                this.$router.push({ name:'Home'})
            }
            console.warn('result', result)
        }
    },
    mounted(){
        let user = localStorage.getItem('user-info')
        if(!user){
            this.$router.push({ name:'Login' })
        }
    }
}
</script>