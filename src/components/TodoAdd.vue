<template>
    <div id="add-container">
        <form @submit="addTodo">
            <input type="text" v-model="name" placeholder="Escribir la tarea">
            <input type="text" v-model="description" placeholder="Escribir descripción">
        </form>
    </div>
</template>

<script>

export default {
    name: 'TodoAdd',
    data(){
        return {
            name: '',
            description:''
        }
    },
    methods:{
        addTodo(e){
            e.preventDefault();
            const newTodo = {
                name: this.name,
                description:this.description,
                completed: false
            };
            this.name = '';
            this.description='';
            this.$emit('add-todo', newTodo);
        }
    }
}
</script>

<style scoped>
    #add-container{
        padding: 10px;
    }
    input{
        padding: 10px;
        outline: none;
        border: solid 1px #ccc;
        width: 100%;
    }
</style>