<template>
  <div class="Card">
    <h1>{{ title.toUpperCase() }}</h1>
    <h3 class="desc">{{ desc }}</h3>
    <select @change="changeStatus" class="option-select">
      <option v-for="(option, key) in options" :key="key" class="options">{{ option }}</option>
    </select>
    <button @click="showForm = !showForm" class="addStatus">{{!this.showForm? "&#x2b;" : "&#10006;"}}</button> <!-- Clicking this button shows the form -->
    <form class="form-status" v-if="showForm">
      <input @input="handleNewStatus" placeholder="new status" v-model="NewStatus" />
      <button type="button" @click="AddNewStatus" >Add</button> <!-- Use a button click to call AddNewStatus -->
    </form>
    <p> &#8635; {{ reversedDate }}</p>
    <div class="pending-footer">
      <!-- <button @click="complete(id)">Start</button> -->
      <button @click="deleteTask(id)" class="button-delete">Delete <i class="material-icons">delete</i></button>
    </div>
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
    status: String,
    id: String,
    options: Array, // Define the options prop
    addOptionMethod: Function, // Define the prop to pass the method
  },
  data: function () {
    return {
      NewStatus: "",
      showForm : false
    };
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
      axios.post(`https://to-do-w2m4.onrender.com/api/v1/task/${id}`)
        .then(() => {
          alert("Task in progress");
        })
        .catch(error => {
          console.error("Error:", error);
          alert("Failed to start task");
        });
    },
    deleteTask(id) {
      axios.delete(`https://to-do-w2m4.onrender.com/api/v1/task/${id}`)
        .then(() => {
          alert("Task deleted successfully");
        })
        .catch(error => {
          console.error("Error:", error);
          alert("Failed to delete task");
        });
    },
    handleNewStatus(e) {
      // e.preventDefault();
      this.NewStatus = e.target.value;
    },
    AddNewStatus() {
      if(this.NewStatus.trim().length == 0){
        alert("new task cannot be empty, setting it back default to 'pending'");
        this.addOptionMethod("pending");
        this.showForm= false;
      }
      else{
      this.addOptionMethod(this.NewStatus); // Use the prop to call the method in the parent
      console.log(this.options);
      this.showForm= false;
      }
    },
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

.desc{
  color: rgb(89, 89, 89);
}
.option-select{
  padding: 1rem;
  border: none;
  border-radius: 1rem;
  color: aliceblue;
  font-weight: 700;
  background-color: rgb(0, 191, 255);
}
.options{
  padding: 1rem;
  color: black;
  font-weight: 700;
}
.form-status{
  display: flex;
  margin: 1rem 0rem;
}

.form-status input{
  height: 2rem;
  border: 1px solid grey;
  padding: 0.5rem;
  border-radius: 10px;
}
.form-status input:focus{
  background: rgb(137, 226, 255);
  color: rgb(106, 106, 106);
  font-weight: 700;
}
.Card:hover {
  box-shadow: 0px 0px 10px grey;
 
  transition: 0.2s ease-in-out;
}
.pending-footer{
  display: flex;
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
.button-delete{
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgb(255, 0, 0)!important;
}

.addStatus{
  background: black!important;
}
.button-delete:hover{
  background: linear-gradient(25deg , #fff, rgb(255, 0, 0));
}
</style>
