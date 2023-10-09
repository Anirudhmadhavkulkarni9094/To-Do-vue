<template>
    <div class="Card">
      <h1>{{ title }}</h1>
      <h5>{{ desc }}</h5>
      <p> &#8635; {{ reversedDate }}</p>
      <button v-on:click="complete(id)">Mark Complete &#10004;</button>
    </div>
  </template>
  
  <script>
import axios from 'axios';

  export default {
    name: "CardComp",
    props: {
      title: String,
      desc: String,
      date: String,
      id : String
    },
    computed: {
      reversedDate() {
        let first8Characters = this.date.substring(0, 10);
        let reversedDate = first8Characters.split("-").reverse().join("-");
        return reversedDate;
      },
    },
    methods: {
      complete(id) {
        axios.post(`https://to-do-w2m4.onrender.com/api/v1/task/complete/${id}`).then(
          alert("Task Completed successfully!")
        )
      },
    },
  };
  </script>
  
  <style>
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
    background: rgb(0, 191, 255);
    color: aliceblue;
    font-weight: 600;
  }
  .Card button:hover {
    background: linear-gradient(20deg, rgb(2, 217, 255), rgb(0, 191, 255));
  }
  </style>
  