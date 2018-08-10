<template>
  <div id="app">
    <h1 class='main-title'><span class="main-title__name" >HILT</span> workout timer</h1>
    <app-setup :timers='timers' :total='totalTime()' :start='startTimer' v-on:startTime='updateTimer($event)' v-if='startTimer'></app-setup>
    <app-start :timers='timers' :total='totalTime()' :beginTimer='beginTimer' v-on:resetGame='reset($event)' v-else></app-start>
    <app-footer></app-footer>
    <app-modal v-on:cancel='closeModal($event)' v-on:restart='restartTime($event)'></app-modal>
  </div>
</template>

<script>
import Setup from "./components/Setup.vue";
import Modal from './components/Modal.vue';
import Footer from "./components/Footer.vue";
import Start from "./components/Start.vue";
import {eventBus} from './main'; 

export default {
  components: {
    appSetup: Setup,
    appFooter: Footer,
    appStart: Start,
    appModal: Modal,  
  },
  data() {
    return {
      startTimer: true,
      beginTimer: false, 
      timers: [
        {
          name: "prep",
          amount: 2
        },
        {
          name: "work",
          amount: 2
        },
        {
          name: "rest",
          amount: 5
        },
        {
          name: "rounds",
          amount: 1
        },
        {
          name: "cool down",
          amount: 20
        },
      ],
    };
  },
  methods: {
    totalTime() {
      var total = 0;
      let rounds = this.timers.filter(item => {
         if(item.name === 'rounds') {
           return item;
        }
      });

      this.timers.map(item => {
        if (item.name === "rest" || item.name === 'work' ) {
          total += item.amount * rounds[0].amount;
        } else if (item.name !== 'rounds') {
            total += item.amount;
        }
      });

       return total;

    },
    updateTimer(value) {
      this.startTimer = value;
      this.beginTimer = true; 
    },

    reset(value) {
      this.startTimer = value; 
      console.log('hello');
    },

  },
  created() {
    eventBus.$on('resetGame', (data) => {
      this.startTimer = data; 
      this.timers[3].amount = 8; 
      this.timers[2].amount = 20; 
    }); 
    eventBus.$on('restart', (data) => {
      this.startTimer = true; 
    })
  }
};


</script >

<style>
body,
html {
  padding: 0;
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #757575;
  margin-top: 0px;
  display: flex;
  align-items: center;
  flex-direction: column;
  min-height: 100vh;
  position: relative;
}

h1,
h2 {
  font-weight: normal;
}

.main-title {
  font-size: 3rem;
  text-transform: capitalize;
  margin-bottom: 2rem;
}

@media screen and (max-width: 600px) {
   .main-title {
     font-size: 2rem;
   }
 }

@media (min-width: 1250px) {
   .main-title {
      font-size: 4rem;
   }
}

.main-title__name {
  color: red;
  font-weight: bold;
  border-bottom: 4px solid black;
  padding: 0px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}


</style>
