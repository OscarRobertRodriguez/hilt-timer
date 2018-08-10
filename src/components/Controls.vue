<template>
  <ul class="controls">
    <li v-if='show'><img class='control pause__img'  @click='[show = !show, stopTime($event.target.alt)]' v-on:modalOpen='stopTime($event)'  src='../assets/pause.svg' alt='pause' /></li>
    <li v-else><img class='control play__img' @click='[show = !show, stopTime($event.target.alt)]' src="../assets/play.png" alt="play"></li>
  </ul>
</template>

<script>
 import { eventBus } from '../main'; 

export default {
  data() {
    return {
      show: true,
    }
  },
  methods: {
    stopTime(el) {
      this.$emit('stopTime', el);
      console.log(el); 
    }
  },
  created(){
    eventBus.$on('modalOpen', (data) => {
          this.stopTime('pause');
          
    });
    eventBus.$on('resumeTime', (data) => {
       this.stopTime('play'); 
    })
    eventBus.$on('cancel', (data) => {
      this.stopTime('play');
    })
  }
}
</script>

<style>
.controls {
  position: absolute;
  top: -2.5rem;
  right: 5px;
}
  .control {
    width: 4rem;
    fill: #fff;
    cursor: pointer;
  }

  @media screen and (min-width: 1200px) {
.controls {
  position: absolute;
  top: -4rem;
  right: 5px;
}
   .control {
     width: 6rem;
   }
 }
</style>


