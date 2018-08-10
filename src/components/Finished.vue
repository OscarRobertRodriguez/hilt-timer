<template>
<div class='finish-card'>
  <h1>Congratulations! You completed your workout.</h1>
  <button class='finish-btn' value='restart' @click.prevent='resetGame'>restart</button>
</div>  
</template>


<script>
 import { eventBus } from '../main'; 
export default {
  methods:{
    resetGame() {
      eventBus.$emit('resetGame', true); 
    }
  }
}
</script>


<style>
  .finish-card {
    border-radius: 5px;
    box-shadow: 5px;
    padding: 1rem 2rem;
    background-color: orangered;
    color: #fff;
    max-height: 70%;
    height: 300px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, .3);
    margin: 0 1rem;
  }

   @media screen and (min-width: 1600px) {
    .finish-card {
      height: 400px;
      font-size: 2rem;
    }
 }

 @media screen and (max-width: 400px) {
   .finish-card {
     height: 300px;
     font-size: 1rem;
      margin: 0 1rem;
      width:200px;
   }
 }


  .finish-btn {
    padding: 1rem 1.5rem;
    color: #fff;
    border: 1px solid #fff;
    background-color: orangered;
    font-size: 2rem;
    text-transform: capitalize;
    transition: box-shadow .2s ease;
    box-shadow: 1px 5px 5px rgba(0, 0, 0, .1); 
  }

  .finish-btn:hover {
    box-shadow: 1px -2px 5px rgba(0, 0, 0, .1);
    cursor: pointer;
  }
</style>


