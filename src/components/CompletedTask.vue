<template>
    <div class="PendingTask Board-element">
      <h1>Finished : {{filteredTasks.length}} tasks</h1>
      <div v-for="(task, index) in filteredTasks" :key="index">
        <CardFinish :title="task.title" :desc="task.Desc" :date="task.Date" :id="task._id" />
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
 
import CardFinish from './CardFinish.vue';
  
  export default {
    name: "CompletedTask",
    components: { CardFinish },
    data() {
      return {
        tasks: []
      };
    },
    computed: {
      filteredTasks() {
        // Filter tasks with "working" status
        return this.tasks.filter(task => task.status === "finished");
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
      }
      
    }
  };
  </script>
  