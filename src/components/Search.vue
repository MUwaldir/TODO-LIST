<template>
    <div id="search-container">
        <input type="text" v-model="query" v-on:input="$emit('query-change', query)" placeholder="Buscar tarea">
    </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data(){
        return {
            query: ''
        }
    }
}
</script>

<style scoped>
    #search-container{
        width: 600px;
        margin: 0 auto;
    }
    input{
        padding: 10px;
        width: 100%;
        outline: none;
        border-radius: 5px;
    }
</style>