<template>
    <div class="PendingTask Board-element">
      <div class="pendingHeader">
        <h1>Pending tasks : {{ filteredTasks.length }}</h1>
        <button class="btn" @click="showForm = true">New Task &#043;</button>
      </div>
      <!-- Show the form when showForm is true -->
      <div v-if="showForm" class="taskForm">
        <form @submit.prevent="add" class="add-form">
          <label for="title">Title:</label>
          <input type="text" id="title" v-model="newTask.title" required  placeholder="Add title"/>
          <br />
          <label for="description">Description: (120 char max)</label>
          <textarea id="description" v-model="newTask.description" required placeholder="Add description" maxlength="120"></textarea>
          <br />
          <div>
              <button class="btn" type="submit">Add Task</button>
              <button class="btn" @click="showForm = false">Cancel</button>
        </div>
        </form>
      </div>
      <div v-for="(task, index) in filteredTasks" :key="index">
        <CardComp :title="task.title" :desc="task.Desc" :date="task.Date" :id="task._id" />
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import CardComp from './Card/CardComp.vue';
  
  export default {
    name: "PendingTask",
    components: { CardComp },
    data() {
      return {
        tasks: [],
        showForm: false,
        newTask: {
          title: "",
          description: ""
        }
      };
    },
    computed: {
      filteredTasks() {
        // Filter tasks with "working" status
        return this.tasks.filter(task => task.status === "pending");
      }
    },
    mounted() {
      this.fetchTasks();
    },
    methods: {
      fetchTasks() {
        axios.get("https://to-do-w2m4.onrender.com/api/v1/task")
          .then(res => {
            this.tasks = res.data;
            console.log(this.tasks);
            this.fetchTasks();
          })
          .catch(error => {
            console.error("Error:", error);
          });
      },
      add() {
        // Implement your logic to handle the form submission here
        // You can access the new task data from this.newTask.title and this.newTask.description
        // Example:
        if(this.newTask.title.trim().length == 0 || this.newTask.description.trim().length == 0){
            alert("Title or description cannot be empty!");
        }
        else{
        axios.post("https://to-do-w2m4.onrender.com/api/v1/task", {
          title: this.newTask.title,
          Desc: this.newTask.description,
          Date: Date.now(),
          status: "pending" // You can set the default status as needed
        })
          .then(response => {
            // Handle the response and reset the form
            console.log(response)
            this.showForm = false;
            // Optionally, refresh the task list
            this.fetchTasks();
          })
          .catch(error => {
            console.error("Error:", error);
          });
      }
    
    }
}
  };
  </script>
  
  <style scoped>
  .add-form{
    display: flex;
    flex-direction: column;
    position: absolute;
    text-align: left;
    color: aliceblue;
    background-color: rgb(0, 0, 0);
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    padding: 1.5rem 1rem;
    width: 20rem;
    height: 15rem;
    justify-content:space-between;
    border-radius: 10px;
  }
  .add-form input{
    padding: 0.5rem 1rem;
  }
  .add-form textarea{
    padding: 0.5rem 1rem;
  }
  .add-form button{
    width: 7rem;
    margin: 0.5rem;
    padding: 0.5rem 1rem;
    border: none;
    background-color: rgb(1, 213, 255);
    border-radius: 10px;
    color: aliceblue;
    font-weight: 600;
  }

  .add-form button:hover{
    background: linear-gradient(25deg,#84efff,rgb(1, 213, 255));
  }
  .pendingHeader {
    display: flex;
    justify-content: space-around;
    margin: 0.5rem;
    align-items: center;
  }
  
  .pendingHeader button {
    margin: 0.3rem;
    padding: 0.5rem;
    border: none;
    border-radius: 10px;
    background: rgb(255, 0, 0);
    color: aliceblue;
    font-weight: 600;
    
  }
  
  .pendingHeader button:hover {
    background: linear-gradient(20deg, rgb(255, 117, 117), rgb(255, 70, 70));
  }
  
  
  </style>
  