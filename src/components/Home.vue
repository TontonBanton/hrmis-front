<template>
    <Header />
    <h1>Welcome{{ name }}, Home Page</h1>
    <table border="1">
        <tr>
            <td>Id</td>
            <td>First Name</td>
            <td>Last Name</td>
            <td>Middle Name</td>
            <td>Actions</td>
        </tr>
        <tr v-for="item in employee" :key="item.id">
            <td>{{ item.id }}</td>
            <td>{{ item.fname }}</td>
            <td>{{ item.lname }}</td>
            <td>{{ item.mname }}</td>
            <td>
                <router-link :to="'/update/' + item.id">Update</router-link>
                <button v-on:click="deleteEmployee(item.id)">Delete</button>
            </td>
        </tr>
    </table>
</template>

<script>
import Header from './Header.vue'
import axios from 'axios'
export default {
    name: 'Home',
    data(){
        return{
            name: '',
            employee: []
        }
    },
    components:{
        Header
    },
    methods:{
        async deleteEmployee(id){
            const result = await axios.delete("http://localhost:3000/employee/"+id) 
            console.warn(result)
            if(result.status == 200){
                this.loadData()
            }
        },
        async loadData(){
            let user = localStorage.getItem('user-info')
            //this.name = JSON.parse(user).name
            if(!user){
                this.$router.push({ name:'Login' })
            }
            let result = await axios.get("http://localhost:3000/employee")
            this.employee = result.data
        }
    },
    async mounted(){
        this.loadData()
    }
}
</script>

<style>
table, td{
    border: 1px solid black;
    border-collapse: collapse;
}
td{
    width: 160px;
    height: 30px;

}
</style>