<template>
<div class='modal__background' v-on:showModal='console.log("whatup")' @click.self='[show = !show, resumeTime()]' :class='{displayBlock: show}'>
  <div class="modal">
    <p class='modal__question'>Are you sure you want to restart?</p>
    <div class='modal__options'>
      <button class='modal__option' @click='restart'>yes</button>
      <button class='modal__option' @click='cancel'>no</button>
    </div>
  </div>
</div>
</template>





<script>
import { eventBus } from "../main";

export default {
  data() {
    return {
     show: false 
    }
  },
  methods: {
    restart() {
      this.show = false;
      eventBus.$emit('restart', true)
    },
    cancel() {
      this.show = false; 
      eventBus.$emit('cancel', true) ;
    },
    resumeTime(){
      eventBus.$emit('resumeTime', true); 
    }
  },
  created() {
    eventBus.$on('modalOpen', (data) => {
       this.show = data; 
    })
  }
};
</script>



<style >
.modal__background {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  overflow: auto;
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.4);
  display: none;
}

.displayBlock {
  display: block; 
}

.modal {
  border-radius: 5px;
  width: 300px;
  background: orangered;
  color: #fff;
  padding: 1.5rem 1rem;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.3);
  margin: 0 auto;
}

.modal__question {
  font-size: 1.5rem;
}
.modal__options {
  color: #fff;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  width: 80%;
  margin: 0 auto;
  letter-spacing: 1px;
}

.modal__option {
  color: #fff;
  border: 3px solid #fff;
  background: transparent;
  font-variant: small-caps;
  padding: 1rem 1.5rem;
  font-size: 1.4rem;
  text-align: center;
  cursor: pointer;
  transition: background-color 0.2s ease, box-shadow 0.1s ease;
  outline: none;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.3);
}

.modal__option:hover {
  background-color: #fff;
  color: red;
}
.modal__option:active {
  box-shadow: 1px 0px 0px rgba(0, 0, 0, 0.3);
}
</style>

