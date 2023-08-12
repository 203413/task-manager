<template>
    <navBar @getTasks="getTasks"/>
    <div class="container">
        <div class="card-header">
        </div>
        <div class="div1">
            <h1>To do</h1>
            <v-table theme="light" density="compact" class="elevation-3">
                <thead>
                    <tr class="header1">
                        <th class="text-left">Task name</th>
                        <th class="text-left">Date</th>
                        <th class="text-left">Options</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in uncompletedTasks" :key="index">
                        <td> <input type="checkbox" :checked="task.is_completed === 1" @change="toggleCompleted(task)"
                                :id="'checkbox_' + index" />
                            {{ task.title }}</td>
                        <td>{{ task.due_date }}</td>
                        <td>

                            <v-row align="center" class="mrn">
                                <edit :propTitle="task.title" :propDate="task.due_date" :propIscompleted="task.is_completed"
                                    :propId="task.id" @getTasks="getTasks" />
                                <v-btn density="comfortable" color="red" @click="$event => deleteTask($event, task.id)">
                                    Delete <v-icon end icon="mdi-cancel"></v-icon>
                                </v-btn>
                            </v-row>
                        </td>
                    </tr>
                </tbody>
            </v-table>
            <h1>Completed</h1>
            <v-table theme="dark" density="compact" class="elevation-3">
                <thead>
                    <tr class="header2">
                        <th class="text-left">Task name</th>
                        <th class="text-left">Date</th>
                        <th class="text-left">Options</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in completedTasks" :key="index">
                        <td> <input type="checkbox" :checked="task.is_completed === 1" @change="toggleCompleted(task)"
                                :id="'checkbox_' + index" />
                            {{ task.title }}</td>
                        <td>{{ task.due_date }}</td>
                        <td>
                            <v-row align="center" class="mrn">
                                <edit :propTitle="task.title" :propDate="task.due_date" :propIscompleted="task.is_completed"
                                    :propId="task.id" @getTasks="getTasks" />
                                <v-btn density="comfortable" color="red" @click="$event => deleteTask($event, task.id)">
                                    Delete <v-icon end icon="mdi-cancel"></v-icon>
                                </v-btn>
                            </v-row>
                        </td>
                    </tr>
                </tbody>
            </v-table>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            tasks: {},
            completedTasks: {},
            uncompletedTasks: {},
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
            token: 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd',
        }
    },
    mounted() {
        this.getTasks();
    },
    methods: {
        getTasks() {
            const url = this.baseURL;
            const token = this.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.get(url, { params: { token: token }, headers: headers })
                .then(response => {
                    console.log('Respuesta:', response.data, response.status);
                    this.tasks = response.data


                    const completedTasks = response.data.filter(task => task.is_completed === 1);
                    const uncompletedTasks = response.data.filter(task => task.is_completed === 0);

                    this.completedTasks = completedTasks;
                    this.uncompletedTasks = uncompletedTasks;
                })
                .catch(error => {
                    console.error('Error:', error);
                });
        },
        deleteTask(event, taskID) {
            console.log('holaaa');
            const url = this.baseURL + '/' + taskID;

            const token = this.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            if (confirm('Seguro que quiere eliminal?')) {
                axios.delete(url, { params: { token: token }, headers: headers })
                    .then(response => {
                        event.target.innerText = 'Delete';
                        console.log('Respuesta:', response.data, response.status);
                        this.getTasks();
                        event.target.innerText = 'Delete';
                    })
                    .catch(error => {
                        console.error('Error:', error);
                    });
            }
        },
        toggleCompleted(task) {
            task.is_completed = task.is_completed === 1 ? 0 : 1;

            const url = this.baseURL + '/' + task.id;
            const token = this.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            const body = {
                token: token,
                title: task.title,
                due_date: task.due_date,
                is_completed: task.is_completed
            }
            axios.put(url, body, { headers })
                .then(response => {
                    console.log('Respuesta:', response.data);
                    this.getTasks();
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            console.log(task.title);
        },
    }
}
</script>

<style>
.mrn {
    margin: 10px;
}

.div1{
    width: 70%;
    margin: 0 auto; 
}

.v-table thead th {
      font-size: 25px !important;
 }

 .header1{
    background-color: black;
 }

 .header1 th{
    color:white !important;
 }

 .header2{
    background-color: white;
 }

 .header2 th{
    color:black !important;
 }
</style>