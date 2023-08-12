<template>
    <div class="custom-container">
        <v-card color="grey-lighten-4" flat height="255px" width="100%" class="round1">
            <v-toolbar collapse>
                <v-btn icon>
                    <v-icon>mdi-github</v-icon>
                </v-btn>

                <v-btn icon>
                    <v-icon>mdi-information</v-icon>
                </v-btn>
            </v-toolbar>
            <h1 class="title1">Task-e manager</h1>
            <h2 class="title2">Create task</h2>

        </v-card>
        <!-- <v-btn size="x-large" icon="mdi-plus" class="custom-button"></v-btn> -->
        <create @greet="greet"/>
        
    </div>
</template>

<script>
export default{
    methods:{
        greet() {
            this.$emit("getTasks");
        },
    }
}
</script>

<style scoped>
.custom-container {
  position: relative;
  height: 255px;
  text-align: center;
}


.round1 {
    border-radius: 0 0 100px 100px;
}

.title1 {
    
    font-weight: 900;
    font-size: clamp(40px, 8vw, 90px);
}

.title2 {
    font-size: 30px;
}

.custom-button {
    position: absolute;
    bottom: -30px;
    /* Ajusta este valor según tu diseño */
    left: 50%;
    transform: translateX(-50%);
}
</style>



<edit :propTitle="task.title" :propDate="task.due_date" :propIscompleted="task.is_completed"
:propId="task.id" @getTasks="getTasks" />