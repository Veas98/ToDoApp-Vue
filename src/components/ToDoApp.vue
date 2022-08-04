<template>
  <div>
    <!-- title -->
    <div id="container">
      <div id="item-1">
        <p class="title">To Do List</p>
      </div>
      <!-- input -->
      <div id="item-2">
        <input type="text" id="inputText" placeholder="Enter Task" v-model="task">
        <div @click="handleAdd" id="add">Add</div>
      </div>
      <!-- table -->
      <table>
        <tr>
          <th class="TaskDescription">Name of task</th>
          <th class="TaskStatus">Status</th>
          <th>###</th>
          <th>###</th>
          <th>###</th>
          <th>###</th>
        </tr>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{task.name}}</td>
          <td @click="handleStatus(index)" :class="{
           'status-to-do': task.status === 'to-do',
           'status-progress': task.status === 'progress',
           'status-done': task.status === 'done',
          }
          ">
            {{ task.status }}
          </td>
          <td @click="handleUp(index)"><i class="bi-arrow-up"></i></td>
          <td @click="handleDown(index)"><i class="bi-arrow-down"></i></td>
          <td @click="handleEdit(index)"><i class="bi-pencil-square"></i></td>
          <td @click="handleDelete(index)"><i class="bi-trash"></i></td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      status: ['to-do', 'progress', 'done'],
      task: '',
      editTask: null,
      tasks: [
        {
          name: "New task",
          status: "to-do",
        },
        {
          name: "Just another one task",
          status: "done",
        },
      ]
    }
  },
  methods: {
    handleAdd() {
      //adding new task
      if (this.editTask != null) {
        this.tasks[this.editTask].name = this.task;
        this.editTask = null;
      } else if (this.task !== '') {
        this.tasks.push({
          name: this.task,
          status: 'to-do',
        });
      }
      this.task = '';
    },

    handleDelete(index) {
      //deleting task
      this.tasks.splice(index, 1);
    },

    handleEdit(index) {
      //editing task
      this.task = this.tasks[index].name;
      this.editTask = index;
    },

    handleStatus(index) {
      //change status
      let statusIndex = this.status.indexOf(this.tasks[index].status);
      statusIndex++;
      if (statusIndex > 2) statusIndex = 0;
      this.tasks[index].status = this.status[statusIndex];
    },

    handleUp(index){
      if (index === 0) return null;
      else{
        let swap = this.tasks[index];
        this.tasks[index] = this.tasks[index - 1];
        this.tasks[index - 1] = swap;
      }
    },

    handleDown(index){
      console.log(this.tasks.length);
      if (index === this.tasks.length - 1) return null;
      else {
        let swap = this.tasks[index];
        this.tasks[index] = this.tasks[index + 1];
        this.tasks[index + 1] = swap;
      }
    },
  }
}
</script>

<style>
body{
  background-color: #363945;
}
#container{
  background-color: #374050;
  display: flex;
  flex-direction: column;
  width: 70vh;
  margin: auto;
  border: 1px solid black;
}
#item-1{
  text-align: center;
}
#item-2{
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
}
tr, td, th {
  border: 1px solid black;
  padding: 10px;
  margin: 5px;
  font-size: 20px;
  text-align: center;
}
.TaskDescription{
  width: 400px;
}
.TaskStatus{
  width: 100px;
}
.title{
  border-bottom: 1px solid black;
  padding: 10px;
  margin-top: 5px;
  font-size: 24px;
}
#inputText{
  background-color: #363945;
  width: 100%;
  padding: 5px;
}
#add{
  padding: 5px;
  cursor: pointer;
  width: 50px;
  text-align: center;
  border-top: 1px solid black;
  border-bottom: 1px solid black;
}
#add:hover{
  background-color: #939597;
}
#add:active{
  transform: translateY(2px);
}
.status-to-do{
  cursor: pointer;
  color: red;
}
.status-progress{
  color: yellow;
}
.status-done{
  color: green;
}
.bi-trash{
  color:#4f0d08;
  cursor: pointer;
}
.bi-pencil-square{
  color: green;
  cursor: pointer;
}
.bi-arrow-down{
  cursor: pointer;
}
.bi-arrow-up{
  cursor: pointer;
}
</style>