<script>

import PendingTask from './PendingTask.vue';

    export default{
    name: "TaskBoard",
    components: { PendingTask }
}
</script>

<template>
    <div class="Board">
        <PendingTask/>
    </div>
</template>

<style scoped>
    .Board{
        display: grid;
        grid-template-columns: 1fr;
        text-align: center;
    }

    @media screen and (max-width : 730px) {
        .Board{
        grid-template-columns: 1fr ;
        }
    }
    @media screen and (max-width : 400px) {
        .Board{
            grid-template-columns: 1fr;
        }
    }
</style>

<style>
button{
    cursor: pointer!important;
}
    .Board-element {
        border: 1px solid rgb(227, 227, 227);
        height: 100vh;
        margin: 0.5rem;
        box-shadow: 0px 0px 5px grey;
        border-radius: 10px;
        color: rgb(0, 0, 0);
        overflow-y: scroll;
        scrollbar-width: none;
    }
    .Board-elementt::-webkit-scrollbar-thumb {
        
        width: 0;
      }
      
      /* Optional: Customize the scrollbar track */
      .Board-element::-webkit-scrollbar {
        width: 0px;
        height: 0px;
        background-color: transparent;
      }

    .Board-element h1{
        font-size: 1.4rem;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
</style>