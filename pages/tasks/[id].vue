<template>
    <div class="mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Edit</h4>
                <NuxtLink to="/">Add task</NuxtLink>
            </div>
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
            taskID: '',
            tTittle: '',
            tDate: '',
            task: {
                token:"e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd",
            },
            isLoading: false,
            isLoadingText: 'Loading',
            baseURL: 'https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks',
        }
    },
    mounted(){
        this.taskID = this.$route.params.id
    },
    methods: {
        updateTask() {
            this.isLoading = true;
            this.isLoadingText = 'Saving'

            if (this.tTittle != ''){
                this.task.title = this.tTittle 
            }
            if (this.tDate != ''){
                this.task.due_date = this.tDate 
            }
            console.log('hola');
            console.log(this.task);

             const url = this.baseURL+'/'+this.taskID;
             const token = this.task.token;
             const headers = {
                 Authorization: `Bearer ${token}`
             };
             axios.put(url,this.task, { headers })
                 .then(response => {
                     console.log('Respuesta:', response.data);
                     this.isLoading = false;
             this.isLoadingText = 'Loading';
                 })
                 .catch(error => {
                     console.error('Error:', error);
                 });
            
            //  this.task.title = '';
            //  this.task.due_date = '';
            //  this.task.description = '';
            //  this.task.comments = '';
            //  this.task.tags = '';


        }
    }
}


</script>

<style>

</style>