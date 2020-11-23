<template>
  <div id="todo-list">
    <div class="container">
    <el-row :gutter="20 ">
      <h1>TO DO LIST</h1>
      <el-col :span="22">
          <div class="grid-content">
              <el-input type="text" v-model="newTask" placeholder="Que hay que hacer hoy?" v-on:keyup.enter="addTask()"></el-input>
              <!-- <input type="text" v-model="newTask" placeholder="Que hay que hacer hoy?" @keyup.enter="addTask()"/> -->
          </div>
      </el-col>
      <el-col :span="2">
          <div class="grid-content">
              <el-button type="info" icon="el-icon-plus" @click="addTask()" circle></el-button>
          </div>
      </el-col>
    </el-row>

    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
            <span>TO DO</span>
        </div>
        <div v-for="(tasks, i) in existingTasks" :key="existingTasks.id" class="item">
            <div class="todo-item-left">
              <el-col :span="5">
                <el-button icon="el-icon-check" circle @click="finishTask(i)"></el-button>
              </el-col>
              <el-col :span="14">
                <div v-if="!tasks.editing" @dblclick="editTask(tasks)" class="todo-item-label">
                    {{ tasks.text }}
                </div>
                <input v-else class="todo-item-edit" type="text" v-model="tasks.text" @blur="finishEdit(tasks)" @keyup.enter="finishEdit(tasks)"  @keyup.esc="cancelEdit(tasks)"v-focus>
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

    <el-row>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
            <span>DONE</span>
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
    return {
    newTask: '',
    taskImportance:'',
    previousTaskText: '',
    existingTasks:[
    {id:0, text: 'Ir al gimnasio', editing: false},
    {id:1, text: 'Preparar el final', editing: false},
    {id:2, text: 'Terminar el ejercicio de programacion', editing: false}
    ],
    completedTask:[],
    }
  },
  methods:{
   addTask(){
    if(this.newTask.trim().length == 0){
        return
    }
    this.existingTasks.push({id: new Date().valueOf(), text: this.newTask}),
    this.newTask = ''
   },
   deleteTask(i){
    this.existingTasks.splice(i,1)
   },
   deleteCompletedTask(i){
    this.completedTask.splice(i,1)
   },
   finishTask(i){
     this.completedTask.push(this.existingTasks[i]),
     this.deleteTask(i)
   },
   editTask(tasks){
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
    font-size: 24px;
    font-family: 'Avenir', Arial, Helvetica, sans-serif;
    text-decoration: line-through;
    color:green;
  }
 
  .todo-item-left{
      display: flex;
      align-items: flex-start;
  }

  .todo-item-label{
      border: 1px solid white;
      font-size: 24px;
      font-family: 'Avenir', Arial, Helvetica, sans-serif;
  }
  .todo-item-edit{
      font-size: 24px;
      color: #2c3e50;
      border: 1px solid #ccc;
      font-family: 'Avenir', Arial, Helvetica, sans-serif;
  }

  .item {
    margin-bottom: 18px;
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

</style>