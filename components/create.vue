<template>
    <v-dialog v-model="dialog" :scrim="false" width="30%" transition="dialog-top-transition">
        <template v-slot:activator="{ props }">
            <div class="custom-button">
                <v-btn size="100" icon="mdi-plus" v-bind="props">
                    <v-icon size="70">mdi-plus</v-icon>
                </v-btn>
                <br>
                <h3 class="titleAdd">Add task!</h3>
            </div>


        </template>
        <v-card>
            <v-toolbar color="black">
                <v-btn icon @click="dialog = false">
                    <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title>Create task</v-toolbar-title>
                <v-spacer></v-spacer>
            </v-toolbar>
            <div class="mt-5">
                <div class="card">
                    <div class="card-body">
                        <div v-if="isLoading">
                            <loading :title="isLoadingText" />
                        </div>
                        <div v-else>
                            <form @submit.prevent="saveTask">
                                <div class="mb-3">
                                    <label>Task</label>
                                    <input type="text" v-model="task.title">
                                </div>
                                <div class="mb-3">
                                    <label>Due date</label>
                                    <input type="text" v-model="task.due_date" placeholder="YYYY-MM-DD">
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
                <v-btn color="blue-darken-1" variant="text" @click="saveTask">
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
            tTittle: '',
            tDate: '',
            task: {
                token: "e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd",
                title: '',
                due_date: '',
                is_completed: 0,
            },
            isLoading: false,
            isLoadingText: 'Loading',
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        }
    },
    methods: {
        saveTask() {
            this.isLoading = true;
            this.isLoadingText = 'Saving'

            const url = 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks';
            const token = 'e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd';
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.post(url, this.task, { headers })
                .then(response => {
                    console.log('Respuesta:', response.data);
                    this.isLoading = false;
                    this.isLoadingText = 'Loading';

                    this.greet2();
                    this.dialog = false;
                })
                .catch(error => {
                    console.error('Error:', error);
                });

            this.task.title = '';
            this.task.due_date = '';


        },
        greet2() {
            this.$emit("greet");
        },
    }
}
</script>
<style scoped>
.dialog-bottom-transition-enter-active,
.dialog-bottom-transition-leave-active {
    transition: transform .2s ease-in-out;
}

.titleAdd {
    font-family: Roboto;
    font-size: 1.75rem;
}



.custom-button {
    position: absolute;
    bottom: -80px;
    left: 50%;
    transform: translateX(-50%);
}
</style>
