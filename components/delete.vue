<template>
    <v-dialog v-model="dialog" :scrim="false" width="30%" transition="dialog-bottom-transition">
        <template v-slot:activator="{ props }">
            <v-btn v-bind="props" color="red" icon density="comfortable" class="mrn">
                <v-icon icon="mdi-cancel" color="white"></v-icon>
            </v-btn>
        </template>
        <v-card>
            <v-toolbar color="red">
                <v-btn icon @click="dialog = false">
                    <v-icon color="white">mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title style="color: white;">Delete task</v-toolbar-title>
                <v-spacer></v-spacer>
            </v-toolbar>
            <div class="card-body">Delete this task?</div>
            

            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="dialog = false">
                    Close
                </v-btn>
                <v-btn color="blue-darken-1" variant="text" @click="deleteTask">
                    Delete
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
            task: {
                token: "e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd",
            },
            isLoading: false,
            isLoadingText: 'Loading',
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        }
    },
    props: {
        propId: String,
    },
    mounted() {
        this.taskID = this.propId
    },
    methods: {
        deleteTask() {
            console.log(this.taskID);
            const url = this.baseURL + '/' + this.taskID;

            const token = this.task.token;
            const headers = {
                Authorization: `Bearer ${token}`
            };
            axios.delete(url, { params: { token: token }, headers: headers })
                .then(response => {
                    console.log('Respuesta:', response.data, response.status);
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
<style scoped>
.dialog-bottom-transition-enter-active,
.dialog-bottom-transition-leave-active {
    transition: transform .2s ease-in-out;
}

.mrn {
    margin: 3px;
}
</style>

