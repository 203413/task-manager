<template>
    <navBar @getTasks="getTasks" />
    <div class="containerMain margin">
        <div class="div1">
            <div class="titleToDo elevation-3">
                <h1>To do</h1>
            </div>

            <v-table theme="light" density="compact" class="elevation-3" :key="tablekey">
                <thead>
                    <tr class="header1">
                        <th class="text-left">Task name</th>
                        <th class="text-left">Date</th>
                        <th class="text-left"></th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in uncompletedTasks" :key="index">
                        <td>
                            <!-- <input type="checkbox" :checked="task.is_completed === 1" @change="toggleCompleted(task)"
                                :id="'checkbox_' + index" /> -->
                            <v-btn icon class="elevation-0"
                                @click="toggleCompleted(task, 1)"><v-icon>mdi-checkbox-blank-outline</v-icon></v-btn>
                            {{ task.title }}
                        </td>
                        <td>{{ task.due_date }}</td>
                        <td>

                            <v-row align="center" class="mrn">
                                <edit :propTitle="task.title" :propDate="task.due_date" :propIscompleted="task.is_completed"
                                    :propId="task.id" @getTasks="getTasks" />
                                <!-- <v-btn density="comfortable" icon color="red" @click="$event => deleteTask($event, task.id)">
                                    <v-icon  icon="mdi-cancel"></v-icon>
                                </v-btn> -->
                                <delete :propId="task.id" @getTasks="getTasks" />
                            </v-row>
                        </td>
                    </tr>
                </tbody>
            </v-table>
            <div class="titleCompleted elevation-3">
                <h1>Finished</h1>
            </div>
            <v-table theme="dark" density="compact" class="elevation-3 bottom" :key="tablekey">
                <thead>
                    <tr class="header2">
                        <th class="text-left">Task name</th>
                        <th class="text-left">Date</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(task, index) in completedTasks" :key="index">
                        <td>
                            <!-- <input type="checkbox" :checked="task.is_completed === 1" @change="toggleCompleted(task)"
                                :id="'checkbox_' + index" /> -->
                            <v-btn icon class="elevation-0"
                                @click="toggleCompleted(task, 0)"><v-icon>mdi-checkbox-marked</v-icon></v-btn>
                            {{ task.title }}
                        </td>
                        <td>{{ task.due_date }}</td>
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
            tablekey: 0,
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
            token: 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd',
        }
    },
    mounted() {
        this.getTasks();
    },
    methods: {
        getTasks() {
            this.completedTasks = {};
            this.uncompletedTasks = {};
            const url = this.baseURL;
            const token = this.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.get(url, { params: { token: token }, headers: headers })
                .then(response => {
                    console.log('Respuesta:', response.data, response.status);
                    this.tasks = response.data;
                    this.completedTasks = response.data.filter(task => task.is_completed === 1);
                    this.uncompletedTasks = response.data.filter(task => task.is_completed === 0);
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
        toggleCompleted(task, value) {
            console.log('DEBUUUUUUG');
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
                is_completed: value
            }
            axios.put(url, body, { headers })
                .then(response2 => {
                    const url = this.baseURL;
                    const token = this.token;
                    const headers = {
                        Authorization: `Bearer ${token}`
                    };
                    axios.get(url, { params: { token: token }, headers: headers })
                        .then(response => {
                            console.log('Respuesta:', response.data, response.status);
                            console.log("DEBUG333");
                            console.log(this.tasks);
                            this.tasks = {};
                            console.log(this.tasks);
                            //this.reRenderChildComponent();
                            this.getTasks();
                        })
                        .catch(error => {
                            console.error('Error:', error);
                        });
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            console.log(task.title);
        },
    }
}
</script>

<style scoped>
.containerMain {
    --bs-gutter-x: 1.5rem;
    --bs-gutter-y: 0;
    width: 100%;
    padding-right: calc(var(--bs-gutter-x) * .5);
    padding-left: calc(var(--bs-gutter-x) * .5);
    margin-right: auto;
    margin-left: auto
        

}

@media (min-width:576px) {

.containerMain{
    max-width: 540px
}
}

@media (min-width:768px) {

.containerMain {
    max-width: 720px
}
}

@media (min-width:992px) {

.containerMain {
    max-width: 960px
}
}

@media (min-width:1200px) {

.containerMain {
    max-width: 1140px
}
}

@media (min-width:1400px) {

.containerMain {
    max-width: 1320px
}
}

.margin {
    margin-top: 120px;
}

.mrn {
    margin: 10px;
}

.div1 {
    width: 70%;
    margin: 0 auto;
}

.titleToDo {
    text-align: center;
    width: 100%;
    background-color: black;
    color: white;
    margin-bottom: 20px;
}

.titleCompleted {
    text-align: center;
    width: 100%;
    background-color: white;
    color: black;
    border-color: black;
    margin-top: 50px;
    margin-bottom: 20px;
}



.v-table thead th {
    font-size: 25px !important;
}

.header1 {
    background-color: black;
}

.header1 th {
    color: white !important;
}

.header2 {
    background-color: white;
}

.header2 th {
    color: black !important;
}

.bottom {
    margin-bottom: 40px;
}
</style>