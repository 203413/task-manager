<template>
    <v-dialog v-model="dialog" :scrim="false" width="30%" transition="dialog-bottom-transition">
        <template v-slot:activator="{ props }">
            <v-btn v-bind="props" color="cyan" icon density="comfortable" class="mrn">
                <v-icon icon="mdi-pencil" color="white"></v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-toolbar  color="cyan">
                <v-btn icon  @click="dialog = false">
                    <v-icon color="white">mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title style="color: white;">Edit task</v-toolbar-title>
                <v-spacer></v-spacer>
            </v-toolbar>
            <div class="mt-5">
                <div class="card">
                    <div class="card-body">
                        <div v-if="isLoading">
                            <loading :title="isLoadingText" />
                        </div>
                        <div v-else>
                            <form @submit.prevent="updateTask">
                                <div class="mb-3">
                                    <label>Title</label>
                                    <input type="text" v-model="tTittle" class="form-control">
                                </div>
                                <div class="mb-3">
                                    <label>due due_date</label>
                                    <input type="text" v-model="tDate" class="form-control">
                                </div>
                                <!-- <div class="mb-3">
                                    <button type="submit" class="btn btn-primary">Save</button>
                                </div> -->
                            </form>
                        </div>
                    </div>
                </div>
            </div>

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="dialog = false">
                    Close
                </v-btn>
                <v-btn color="blue-darken-1" variant="text" @click="updateTask">
                    Save
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>
<script>
import axios from 'axios';
export default {
    data() {
        return {
            dialog: false,
            notifications: false,
            sound: true,
            widgets: false,
            taskID: '',
            tTittle: '',
            tDate: '',
            task: {
                token: "e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd",
            },
            isLoading: false,
            isLoadingText: 'Loading',
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        }
    },
    props: {
        propTitle: String,
        propDate: String,
        propIscompleted: String,
        propId: String,
    },
    mounted() {
        this.taskID = this.propId
        this.tTittle = this.propTitle
        this.tDate = this.propDate
    },
    methods: {
        updateTask() {
            this.isLoading = true;
            this.isLoadingText = 'Saving'
            this.task.title = this.tTittle
            this.task.due_date = this.tDate
            this.task.is_completed = this.propIscompleted
            console.log('hola');
            console.log(this.task);

            const url = this.baseURL + '/' + this.taskID;
            const token = this.task.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.put(url, this.task, { headers })
                .then(response => {
                    console.log('Respuesta:', response.data);
                    this.isLoading = false;
                    this.isLoadingText = 'Loading';
                    this.dialog = false
                    this.greet();
                })
                .catch(error => {
                    console.error('Error:', error);
                });


        },
        greet() {
            this.$emit("getTasks");
        },
    }
}
</script>
<style>
.dialog-bottom-transition-enter-active,
.dialog-bottom-transition-leave-active {
    transition: transform .2s ease-in-out;
}

.mrn {
    margin: 3px;
}
</style>

