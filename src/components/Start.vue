<template>
<div class='start'>
  <app-start-card v-if='prepPhase'>
    <span>{{prep.name}}</span>
    <p>{{this.timers[0].amount}}</p>
  </app-start-card>
  <app-start-card v-else v-show='test'>
    <span>{{work.name}}</span>
    <p>{{this.timers[1].amount}}</p>
  </app-start-card>
    <app-start-card v-show='restPhase'>
    <span>{{rest.name}}</span>
    <p>{{this.timers[2].amount}}</p>
  </app-start-card>
    <app-start-card v-show='coolPhase' style='background-color: lightBlue'>
    <span>{{cool.name}}</span>
    <p>{{this.timers[4].amount}}</p>
  </app-start-card>
  <app-start-card v-show='roundPhase'>
    <span>{{rounds.name}}</span>
    <p>{{rounds.amount}}</p>
  <app-restart></app-restart>
  </app-start-card>
  <app-start-card v-show='totalPhase'>
    <span>total time</span>
    <p>{{totalTest}}</p>
    <app-controls v-on:stopTime='pauseTime($event)' v-on:modalOpen='pauseTime($event)' v-show='!finishWork'></app-controls>
  </app-start-card>
  <app-finish v-show='finishWork'></app-finish>
  
</div>

</template>

<script>
import Go from "../assets/go.wav";
import Rest from "../assets/rest.wav";
import Done from "../assets/done.wav";
import Restart from "./Close.vue"; 
import StartCard from "./StartCard.vue";
import Finish from "./Finished.vue";
import Controls from "./Controls.vue";
import {eventBus} from '../main'; 

export default {
  components: {
    appStartCard: StartCard,
    appFinish: Finish,
    appControls: Controls,
    appRestart: Restart 
  },
  props: ["timers", "total", "beginTimer"],
  data() {
    return {
      soundCounter: 0,
      interval: null,
      prepPhase: true,
      coolPhase: false,
      restPhase: false,
      roundPhase: true,
      totalPhase: true,
      finishWork: false,
      test: true,
      currentTotal: null,
      work: Array.from(this.timers)[1],
      rest: Array.from(this.timers)[2],
      prep: [...this.timers][0],
      rounds: Array.from(this.timers)[3],
      cool: Array.from(this.timers)[4],
      totalTest: this.total
    };
  },
  methods: {
    prepCountDown() {
      var store = this.prep.amount;
      this.interval = setInterval(() => {
        this.timers[0].amount--;
        this.totalTest--;
        if (this.timers[0].amount == 0) {
          clearInterval(this.interval);
          this.timers[0].amount = store;
          this.prepPhase = false;
          this.workCountDown();
        }
      }, 1000);
    },
    workCountDown() {
      var store = this.work.amount;
      let GoSound = new Audio(Go);
      if (this.soundCounter === 0) {
        GoSound.play();
      }
      this.interval = setInterval(() => {
        this.soundCounter = 0;
        this.timers[1].amount--;
        this.totalTest--;
        if (this.timers[1].amount == 0) {
          clearInterval(this.interval);
          this.timers[1].amount = store;
          this.test = false;
          this.restPhase = true;
          this.restCountDown();
        }
      }, 1000);
    },
    restCountDown() {
      var store = this.rest.amount;
      let RestFx = new Audio(Rest);
      if(this.soundCounter === 0) {
       RestFx.play();
      }
      this.interval = setInterval(() => {
        this.soundCounter = 0 ;
        this.timers[2].amount--;
        this.totalTest--;
        if (this.timers[2].amount == 0 && this.rounds.amount > 1) {
          clearInterval(this.interval);
          this.timers[2].amount = store;
          this.test = true;
          this.restPhase = false;
          this.rounds.amount--;
          this.workCountDown();
        }
        if (this.timers[2].amount == 0 && this.rounds.amount == 1) {
          clearInterval(this.interval);
          this.rounds.amount = 0;
          this.coolPhase = true;
          this.test = false;
          this.restPhase = false;
          this.coolCountDown();
        }
      }, 1000);
    },
    coolCountDown() {
      var store = this.cool.amount;
      let DoneFx = new Audio(Done); 
      if (this.soundCounter === 0) {
        DoneFx.play();
      }

      this.interval = setInterval(() => {
        this.timers[4].amount--;
        this.totalTest--;
        if (this.timers[4].amount == 0) {
          clearInterval(this.interval);
          this.timers[4].amount = store;
          this.coolPhase = false;
          this.roundPhase = false;
          this.totalPhase = false;
          this.finishWork = true;
        }
      }, 1000);
    },
    pauseTime(value) {
      if (value === "pause" ) {
        clearInterval(this.interval);
      }
      if (
        !this.restPhase &&
        value === "play" &&
        this.coolPhase === false &&
        this.prepPhase === false
      ) {
        this.soundCounter = 1; 
        this.workCountDown();
      }
      if (this.restPhase && value === "play") {
        this.soundCounter = 1; 
        this.restCountDown();
      }
      if (this.coolPhase && value === "play") {
        this.soundCounter = 1; 
        this.coolCountDown();
      }
      if (this.prepPhase && value === "play") {
        this.prepCountDown();
      }
    }
  },
  mounted() {
    console.log("oscar");
    this.prepCountDown();
  },
  created() {
    eventBus.$on('modalOpen', (data) => {
        console.log('ddddd');
    })
  }
};
</script>


<style >
.start {
  margin: 0 auto;
  display: flex;
  flex: 1 0 auto;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: relative;
}
.start-card {
  width: 300px;
  height: 150px;
  margin: 0 auto;
  margin-bottom: 1rem;
  background-color: orangered;
  color: #fff;
  font-size: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  overflow: hidden;
  border-radius: 5px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);
  position: relative;
}

@media screen and (min-width: 1200px) {
  .start-card {
    width: 600px;
    height: 250px;
    font-size: 5rem;
  }
}

.start-card span {
  text-align: left;
  width: 100%;
  display: block;
  font-size: 2rem;
  text-transform: capitalize;
  padding-left: 1rem;
}

@media screen and (min-width: 1200px) {
  .start-card span {
    font-size: 3rem;
  }
}

.start-card p {
  margin: 0;
  padding-bottom: 1rem;
}
</style>


