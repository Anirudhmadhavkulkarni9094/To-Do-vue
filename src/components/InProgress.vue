<template>
    <div class="InProgress Board-element">
      <h1>InProgress : {{filteredTasks.length}}</h1>
      <div v-for="(task, index) in filteredTasks" :key="index">
        <CardProgress :title="task.title" :desc="task.Desc" :date="task.Date" :id="task._id" />
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import CardProgress from './CardProgress.vue';
  
  export default {
    name: "InProgress",
    components: { CardProgress },
    data() {
      return {
        tasks: []
      };
    },
    computed: {
      filteredTasks() {
        // Filter tasks with "working" status
        return this.tasks.filter(task => task.status === "working");
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
            this.fetchTasks()
          })
          .catch(error => {
            console.error("Error:", error);
          });
      }
    }
  };
  </script>
  