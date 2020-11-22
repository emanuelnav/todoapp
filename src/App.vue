<template>
  <div id="app">
    <div class="container">
      <h1>TO DO LIST</h1>
      <input class="input" v-model="newTask"/>
      <select class="importance" v-model="taskImportance">
        <option value="MI">Muy importante</option> 
        <option value="I">Importante</option>
        <option value="NI">No tan importante</option>
      </select>
      <button class="add-button" v-on:click="add()">Add</button>
      <ul>
        <li v-for="(tasks,i) in existingTasks">
          <span class="important">
            {{i+1}}: {{tasks.text}}
          </span>
          <button class="task-button" @click="deleteTask(i)">X</button>
          <button class="task-button" @click="finishTask(i)">C</button> 
        </li>
      </ul>
      <h1>DONE</h1>
      <ul>
        <li v-for="task in completedTask">
          <span class="completedTask">
            {{task.text}}
          </span>
        </li>  
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    newTask: '',
    taskImportance:'',
    existingTasks:[
    {text: 'Ir al gimnasio', id:0, importance:'MI'},
    {text: 'Preparar el final ', id:1, importance:'I'},
    {text: 'Terminar el ejercicio de programacion', id:2, importance:'NI'}
    ],
    completedTask:[]
    }
  },
  methods:{
   add(){
    this.existingTasks.push({text: this.newTask, id: new Date().valueOf(), importance: this.taskImportance}),
    this.newTask = ''
   },
   deleteTask(i){
    this.existingTasks.splice(i,1)
   },
   finishTask(i){
     this.completedTask.push(this.existingTasks[i]),
     this.deleteTask(i)
   }
  }
}
</script>

<style>
  .completedTask{
    text-decoration: line-through;
    color:green;
  }
  .veryImportant{
    color: red;
  }
  .important{
    color:orange;
  }
  .notImportant{
    color: greenyellow;
  }
</style>
