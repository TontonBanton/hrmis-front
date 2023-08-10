<template>
    <Header />
    <h1>Update Employee Page</h1>
    <form class="add">
        <input type="text" name="fname" placeholder="First Name" v-model="employee.fname">
        <input type="text" name="lname" placeholder="Last Name" v-model="employee.lname">
        <input type="text" name="mname" placeholder="Middle Name" v-model="employee.mname">
        <button type="button" v-on:click="updateEmployee">Update Employee</button>
    </form>
</template>

<script>
import Header from './Header.vue'
import axios from 'axios'
export default {
    name: 'Update',
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
        async updateEmployee(){
            console.warn(this.employee)
            const result = await axios.put('http://localhost:3000/employee/' + this.$route.params.id, 
            {
                fname: this.employee.fname,
                lname: this.employee.lname,
                mname: this.employee.mname
            })
            if(result.status == 200){
                this.$router.push({ name:'Home'})
            }
            console.warn('result', result)
        }
    },
    async mounted(){
        let user = localStorage.getItem('user-info')
        if(!user){
            this.$router.push({ name:'Login' })
        }
        const result = await axios.get('http://localhost:3000/employee/' + this.$route.params.id)
        //console.warn(this.$route.params.id)
        console.warn(result.data)
        this.employee = result.data
    }
}
</script>