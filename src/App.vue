<template>
  <div id="container">
     <h1>ToDo list</h1>
      <input v-model="newTodo" />
      <button class = "waves-effect waves-light btn" @click="addTodos()">Add a task</button>
      <button class="waves-effect waves-light btn red lighten-2 deleteBtn" @click="reSetTodos()" >Remove All todos</button>
    
     <div id="todoList">
        <div :class="{completed:todo.completed}" v-for="(todo, index) in todos" :key = "index" >
            <div class="card teal teal cardConfig" v-if = "index > 0">
                  <span class="card-title">{{ todo.text }}</span>
                  <button class="waves-effect waves-light btn red lighten-2 deleteTodoBtn" @click="deleteTodo(index)" >Remove task</button>
            </div>
        </div>

     </div>
  </div>
</template>

<script>
import {ref} from "vue";


export default {
  name: 'App',
  components: {
    
  }, 
  setup(){
    let newTodo = ref("");
    const initialLoadData = [
      {
        complete : "false",
        text : ''
      }
    ];
    let storedTodos;
    
    localStorage.getItem("storedTodos") ? (storedTodos = JSON.parse(localStorage.getItem("todos")))
    : (storedTodos = initialLoadData)
    
    const todos = ref(storedTodos);

    function addTodos(){
      if(newTodo.value !== ""){
        todos.value.push({
          complete : false, 
          text : newTodo.value
        });
       
        this.newTodo = "";
      }
    }

    function reSetTodos(){
      this.newTodo = "";
      todos.value.splice(0,todos.value.length);
    }

    function deleteTodo(index){
      todos.value.splice(index, 1);
    }

    return {
      addTodos, todos, newTodo, reSetTodos, deleteTodo
    }

  },  

  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.deleteBtn {
  margin-left: 1em;
}

.deleteTodoBtn{
  display: flex;
  flex-direction: row;
  flex-wrap : wrap;
  margin-right: 2em;
}

.cardConfig {
  width: 50%;
  margin: 1em auto;
}

</style>
