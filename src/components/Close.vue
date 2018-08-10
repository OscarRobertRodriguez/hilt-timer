<template>
  <div class='restart'>
    <img src="../assets/close.svg" alt="close button" @click='showModal'>
  </div>
</template>

<script>
import {eventBus} from '../main'; 
export default {
  data() {
    return {
    }
  },
  methods: {
    showModal() {
      eventBus.$emit('modalOpen', true); 
    }
  }
}
</script>

<style>
   
   .restart {
     position: absolute;
     top: 5px;
     right: 2rem;
   }

   .restart img {
     width: 4rem;
     cursor: pointer;
   }



 @media screen and (max-width: 1199px) {
   .restart {
     right: 1.5rem;
   }
   .restart img { 
     width: 2.5rem;
   }
 }
</style>
