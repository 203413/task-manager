<template>
    <div>
        <div class="card-header">
            <h5>Taks list
                <NuxtLink to="/create">Add task</NuxtLink>
            </h5>
        </div>
        <div class="card-body">
            <table class="table table-striped">
                <thead >
                    <tr>
                        <th>Completed</th>
                        <th>Title</th>
                        <th>date</th>
                    </tr>
                </thead>
            
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td>{{ task.is_completed }}</td>
                    <td>{{ task.title }}</td>
                    <td>{{ task.due_date }}</td>
                    <td><NuxtLink to="/" class="btn btn-success btn-sm mx-2">Edit</NuxtLink></td>
                    <td><button  class="btn btn-danger btn-sm mx-2">Delete</button></td>
                </tr>
            </tbody>
        </table>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            tasks: {},
        }
    },
    mounted() {
        this.getTasks();
    },
    methods: {
        getTasks() {
            const url = 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks';
            const token = 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.get(url, { params:{token: token},headers:headers })
                .then(response => {
                    console.log('Respuesta:', response.data, response.status);
                    this.tasks = response.data
                })
                .catch(error => {
                    console.error('Error:', error);
                });
        }
    }
}
</script>

<style></style>