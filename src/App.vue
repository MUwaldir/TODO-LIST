

<!-- implemenmtacion en graphql -->
<template>
  <div id="app">
    <div id="box-input">

      <div id="inputs-todo">
  
        <h1>Aplicación de Gestión de Tareas</h1>
        <input type="text" v-model="name" placeholder="Tarea nombre"  />
        <input type="text" v-model="description" placeholder="Tarea descripción" />
        <button class="btn-crear" v-on:click="createTodo">Crear Tarea</button>
      </div>
    </div>
    <div id="box-todos" >
      <div id="box-todo" v-for="item in todos" :key="item.id">
        <div v-bind:class="item.completed?'class-check':''">
          <input type="checkbox" v-bind:checked="item.completed" v-on:change="updateTodo(item.id,item.completed)" >
          
          <h3 class="name">{{ item.name }}</h3>
          <p>{{ item.description }}</p>
        </div>
        <div class="box-btn">
  
          <button id="btn-eliminar" v-on:click="deleteTodo(item.id)">Eliminar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { API } from 'aws-amplify';
  import { createTodo, deleteTodo,updateTodo } from './graphql/mutations';
  import { listTodos } from './graphql/queries';

  export default {
    name: 'App',
    async created() {
      this.getTodos();
    },
    data() {
      return {
        name: '',
        description: '',
        completed:false,
        todos: []
      };
    },
    methods: {
      async createTodo() {
        const { name, description,completed } = this;
        if (!name || !description) return;
        const todo = { name, description,completed };
        this.todos = [...this.todos, todo];
        await API.graphql({
          query: createTodo,
          variables: { input: todo }
        });
        this.name = '';
        this.description = '';
      },
      async getTodos() {
        const todos = await API.graphql({
          query: listTodos
        });
        this.todos = todos.data.listTodos.items;
      },
      async deleteTodo(id) {
        const todo = {
          id:id
        }
        await API.graphql({
          query:deleteTodo,
          variables:{
            input:todo
          }
        })
      
        const todos = await API.graphql({
        query: listTodos
        });
        this.todos = todos.data.listTodos.items;
          
      },
     async updateTodo(id,com){
            const todo = {
              id:id,
              completed:!com
            };
            await API.graphql({
              query:updateTodo,
              variables :{
                input:todo
              }
            });
        const todos = await API.graphql({
        query: listTodos
        });
        this.todos = todos.data.listTodos.items;
      }
    
    
    }
    
  };
</script>

<style>

#inputs-todo{
  width: 600px;
  margin: 0 auto;
}

#inputs-todo input{
  padding: 10px;
  width: 100%;
  outline: none;
}

#box-input{
  font-weight: bold;
  color: #fff;
  background:  black;
  padding: 10px;
}
#box-todos{
  border: solid 1px #ccc;
  width: 600px;
  margin: 10px auto;
}

#box-todo{
  border-bottom: solid 1px #ccc;
  padding: 10px;
  display: flex;
  justify-content: space-between;
}
.name{
  font-weight: bold;
}

.box-btn{
  
  padding: 0 10px;
  width: 15%;
  
}

.btn-crear{
  background: green;
  color: #fff;
  font-weight: bold;
  border-radius: 5px;
  padding: 6px;
}
#btn-eliminar{
  border: none;
  border-radius: 3px;
  padding: 10px;
  background-color: #ccc;
  color: black;
}
#btn-eliminar:hover{
  background-color: #da2020;
  color: white;
}

.class-check{
  text-decoration: line-through;
  color: #ccc;
}
</style>