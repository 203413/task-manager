<template>
    <div class="mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Add task</h4>
                <NuxtLink to="/">Add task</NuxtLink>
            </div>
            <div class="card-body">
                <div v-if="isLoading">
                    <loading :title="isLoadingText" />
                </div>
                <div v-else>
                    <form @submit.prevent="saveTask">
                        <div class="mb-3">
                            <label>Title</label>
                            <input type="text" v-model="task.title" class="form-control">
                        </div>
                        <div class="mb-3">
                            <label>due due_date</label>
                            <input type="text" v-model="task.due_date" class="form-control">
                        </div>
                        <div class="mb-3">
                            <button type="submit" class="btn btn-primary">Save</button>
                        </div>

                    </form>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import axios from 'axios';
export default {
    name: 'Hola',
    data() {
        return {
            task: {
                token:"e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd",
                title: "",
                is_completed: "0",
                due_date: "",
                comments: "",
                description: "",
                tags: "",
            },
            isLoading: false,
            isLoadingText: 'Loading'
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
            axios.post(url,this.task, { headers })
                .then(response => {
                    console.log('Respuesta:', response.data);
                    this.isLoading = false;
            this.isLoadingText = 'Loading';
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            
            this.task.title = '';
            this.task.due_date = '';
            this.task.description = '';
            this.task.comments = '';
            this.task.tags = '';


        }
    }
}


</script>