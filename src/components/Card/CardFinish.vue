<template>
    <div class="Card">
      <h1>{{ title }}</h1>
      <h5>{{ desc }}</h5>
      <p> &#8635; {{ reversedDate }}</p>
      <button v-on:click="deleteTask(id)" class="button-delete">Delete <i class="material-icons">delete</i></button>
    </div>
  </template>
  
  <script scoped>
  import axios from 'axios';
  
  export default {
    name: "CardComp",
    props: {
      title: String,
      desc: String,
      date: String,
      id: String
    },
    computed: {
      reversedDate() {
        let first8Characters = this.date.substring(0, 10);
        let reversedDate = first8Characters.split("-").reverse().join("-");
        return reversedDate;
      },
    },
    methods: {
      deleteTask(id){
        axios.delete(`https://to-do-w2m4.onrender.com/api/v1/task/${id}`)
          .then(() => {
            alert("Task deleted successfully");
          })
          .catch(error => {
            console.error("Error:", error);
            alert("Failed to delete task");
          });
      }
    },
  };
  </script>
  
  <style scoped>
  .Card {
    border: 1px solid rgb(220, 220, 220);
    text-align: left;
    padding: 1rem;
    margin: 1rem;
    border-radius: 10px;
    box-shadow: 0px 0px 5px grey;
    color: black;
  }
  .Card:hover {
    box-shadow: 0px 0px 10px grey;
    transition: 0.2s ease-in-out;
  }
  .Card button {
    margin: 0.3rem;
    padding: 0.7rem 1.1rem;
    border: none;
    border-radius: 10px;
    background: rgb(255, 0, 0);
    color: aliceblue;
    font-weight: 600;
  }
  
  .Card button:hover {
    background: linear-gradient(20deg, rgb(255, 94, 94), rgb(255, 1, 1));
  }

  .button-delete{
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgb(255, 0, 0);
  }
  </style>
  