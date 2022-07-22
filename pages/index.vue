<template>
  <v-app>
    <v-container class="d-flex flex-column align-center">
      <h1>Moses' To-do App!</h1>
      <span class="d-block green--text">Best todo application available in the market!</span>
      <span class="font-weight-bold text-decoration-underline d-block green--text" v-if="tasks.length !== 0">Total Items: {{ tasks.length }} / 10</span>
      <span class="font-weight-bold text-decoration-underline d-block green--text" v-if="tasks.length === 0">Total Items: No tasks!</span>
      <div class="tasks-container mt-10 pa-5 d-flex flex-column align-center">
        <div class="item d-flex pt-5 pb-5" v-for="task in tasks" :key="tasks.indexOf(task)">
          <v-icon class="mr-5">mdi-check</v-icon>
          {{ task.content }}
          <v-icon class="ml-auto"  @click="editItem(tasks.indexOf(task))">mdi-pencil</v-icon> 
          <v-icon class="ml-3" @click="deleteItem(tasks.indexOf(task))">mdi-delete</v-icon> 
        </div>
        <div class="empty-task-list" v-if="tasks.length === 0">No Tasks Yet!</div>
      </div>
      <v-text-field v-if="!limitReached" :error-messages="errors" counter="50" outlined v-model="taskInput" clearable label="Enter Task" class="input-tasks mt-10">Enter new Task here</v-text-field>
      <v-text-field v-if="limitReached" counter="50" disabled outlined v-model="task" clearable label="Enter Task" class="input-tasks mt-10">Enter new Task here</v-text-field>
      <v-btn v-if="!limitReached" @click="addTask" class="add-task mt-3 green white--text">{{ isOnEditMode ? 'Edit' : 'Add Item' }} </v-btn>
      <v-btn v-if="limitReached" disabled @click="addTask()" class="add-task mt-3 green white--text">Add Item</v-btn>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data(){
    return {
      tasks: [],
      limitReached:false,
      taskInput: '',
      errors: [],
      isOnEditMode:false,
      currentKeyOnEdit: null
    }
  },
  methods: {
    addTask(){
      const key = this.currentKeyOnEdit
      if(this.isOnEditMode){
        if(this.taskInput === ''){
          this.errors = ['You cannot have an empty input!']
        }else{
          this.tasks[key].content = this.taskInput
          this.taskInput = ''
          this.errors = []
          this.isOnEditMode = false
        }
      }else{
        if(this.taskInput === ''){
          this.errors = ['You cannot have an empty input!']
        }else{
          this.errors = []
          this.tasks.unshift({content: this.taskInput})
          this.taskInput = ''
          this.limitReached = this.tasks.length >= 10
        }
      }
    },
    deleteItem(key){
      const allItems = this.tasks
      allItems.splice(key, 1)
      this.tasks = allItems
      this.limitReached = this.tasks.length >= 10
    },
    editItem(key){
      this.isOnEditMode = true
      this.taskInput = this.tasks[key].content
      this.currentKeyOnEdit = key
    }
  }
}
</script>

<style>
  *::-webkit-scrollbar {
    width: 10px;
    background-color: #ccc;
    border-radius: 5px;
  }

  *::-webkit-scrollbar-thumb {
    cursor: pointer;
    background-color: green;
    border-radius: 5px;
  }
  .tasks-container > .item{
    border: solid 1px #eee;
    padding: 0 10px;
    border-radius: 5px;
    width: 300px;
    cursor: pointer;
  }
  .empty-task-list{
    font-weight: 900;
    font-size: 1.5em;
    width: 300px;
  }

  .tasks-container > .item:hover{
    border-color: #00008b;
  }
  .input-tasks{
    padding: 10px;
    width: 300px;
  }

  .tasks-container{
    gap: 10px;
    height: 50vh;
    overflow-y: scroll;
  }
  .add-task{
    width: 300px;
    text-align: center;
  }
</style>
