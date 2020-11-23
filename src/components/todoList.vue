<template>
  <div id="todo-list">
    <div class="container">
    <!--Primera fila donde el usuario carga las tareas-->
    <el-row :gutter="20">
      <h1 class="typography">TO DO LIST</h1>
      <el-col :span="22">
          <div class="grid-content">
              <input type="text" class="input-task" v-model="newTask" placeholder="Que hay que hacer hoy?" @keyup.enter="addTask()"/>
          </div>
      </el-col>
      <el-col :span="2">
          <div class="grid-content">
              <el-button type="info" icon="el-icon-plus" @click="addTask()" circle></el-button>
          </div>
      </el-col>
    </el-row>

    <!--Segunda fila en donde se muestran las tareas a realizar-->
    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
            <span class="typography">TO DO</span>
        </div>
        <div v-for="(tasks, i) in existingTasks" :key="existingTasks.id">
            <div class="todo-item">
              <el-col :span="5">
                <el-button icon="el-icon-check" circle @click="finishTask(i)"></el-button>
              </el-col>
              <el-col :span="14">
                <!--Si editing = false(por defecto), muestra las tareas  -->
                <div v-if="!tasks.editing" @dblclick="editTask(tasks)" class="todo-item-label">
                    {{ tasks.text }}
                </div>
                <!--En caso contrario, se muestra un input para que el usuario pueda modificar la tarea-->
                <input v-else class="todo-item-edit" type="text" v-model="tasks.text" @blur="finishEdit(tasks)" @keyup.enter="finishEdit(tasks)"  @keyup.esc="cancelEdit(tasks)" v-focus>
              </el-col>
              <el-col :span="5">
                <el-popconfirm confirm-button-text='Si' cancel-button-text='No' icon="el-icon-info" icon-color="red" title="Estas seguro?" @confirm="deleteTask(i)">
                    <el-button slot="reference" type="danger" icon="el-icon-delete" circle></el-button>
                </el-popconfirm>  
              </el-col>
            </div>
        </div>
      </el-card>
    </el-row>

    <!--Tercera fila donde se muestran las tareas marcadas como terminadas-->
    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
            <span class="typography">DONE</span>
        </div>
        <div v-for="task in completedTask">
            <el-col :span="22" class="completedTask">
                {{ task.text }}
            </el-col>
            <el-col :span="2">
                <el-button type="danger" icon="el-icon-delete" circle @click="deleteCompletedTask(i)"></el-button>
            </el-col>
        </div>
      </el-card>
    </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {    //variables
    newTask: '',
    taskImportance:'',
    previousTaskText: '',
    existingTasks:[],
    completedTask:[],
    }
  },
  methods:{
   addTask(){  //Esta funcion agrega una nueva tarea a la lista de tareas
    if(this.newTask.trim().length == 0){
        return
    }
    this.existingTasks.push({id: new Date().valueOf(), text: this.newTask, editing: false}),
    this.newTask = ''
   },
   deleteTask(i){  //Esta funcion borra una tarea que no esta terminada
    this.existingTasks.splice(i,1)
   },
   deleteCompletedTask(i){ //Esta funcion borra una tarea terminada
    this.completedTask.splice(i,1)
   },
   finishTask(i){  //Esta funcion marca una tarea como terminada
     this.completedTask.push(this.existingTasks[i]),
     this.deleteTask(i)
   },
   editTask(tasks){ //Esta funcion permite al usuario editar una tarea
    this.previousTaskText = tasks.text
    tasks.editing = true    
   },
   finishEdit(tasks){
    if(tasks.text.trim().length == 0){
        tasks.text = this.previousTaskText
    }
    tasks.editing = false
   },
   cancelEdit(tasks){
    tasks.text = this.previousTaskText
    tasks.editing = false
   }
  },
  directives: {
    focus: {
        // Definición de directiva
        inserted: function (el) {
        el.focus()
        }
    }
  }
}
</script>

<style>
  .container{
    max-width: 600px;
    margin: 0 auto;
  }
  .completedTask{
    padding: 10px;
    margin-bottom: 15px;
    font-size: 22px;
    font-family: "Helvetica Neue",Arial, Helvetica, sans-serif;
    text-decoration: line-through;
    color:green;
  }
 
  .todo-item{
    display: flex;
    align-items: flex-start;
    margin-bottom: 18px;
  }

  .todo-item-label{
      border: 1px solid white;
      font-size: 22px;
      font-family: "Helvetica Neue",Arial, Helvetica, sans-serif;
  }
  .todo-item-edit{
      font-size: 22px;
      color: #2c3e50;
      border: 1px solid #ccc;
      font-family: "Helvetica Neue",Arial, Helvetica, sans-serif;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 600px;
  }
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .input-task{
    width: 100%;
    padding: 10px 0px 10px 10px;
    
  }
  .typography{
    font-family: "Helvetica Neue",Arial, Helvetica, sans-serif;
  }

</style>