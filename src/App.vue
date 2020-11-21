<template>
  <div id="app">
    <div class="container">
      <h1>TO DO LIST</h1>
      <input class="input" v-model="newTask"/>
      <button class="add-button" v-on:click="add()">Add</button>
      <ul>
        <li v-for="(tasks,i) in existingTasks">
          <span>
            {{i+1}}: {{tasks.text}}
          </span>
          <button class="task-button" @click="deleteTask(i)">X</button>
          <button class="task-button" @click="finishTask(i)">C</button> 
        </li>
      </ul>
      <h1>DONE</h1>
      <ul>
        <li v-for="task in completedTask">
          <span>
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
    existingTasks:[
    {text: 'Ir al gimnasio', id:0},
    {text: 'Preparar el final ', id:1},
    {text: 'Terminar el ejercicio de programacion', id:2}
    ],
    completedTask:[]
    }
  },
  methods:{
   add(){
    this.existingTasks.push({text: this.newTask, id: new Date().valueOf()}),
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

</style>
