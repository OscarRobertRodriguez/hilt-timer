
<template>
<div class='setup-container'>
  <ul class='setup'>
       <li class="setup__phase" v-for='(time,index) in timers' :key='index'>
        <div class='setup__display'>
          <input class='setup__input' type='text'  :value="time.amount > 0 ? time.amount : 0" @keyup="addValue($event, index)"  >
          <span>{{time.name}}</span>
        </div>
          <div class='adjusters'>
            <button value='-' @click='time.amount > 0 ? time.amount-- : 0 '>&minus;</button>
            <button value='+' @click='time.amount++'>&#x2b;</button>
          </div>
      </li>
    </ul>
    <div class='setup__total'>
      <input class='setup__input' type="text" :value='total'>
      <span>Total</span>
    </div>
<button class='submit' @click.prevent='startTime'>Go</button>
</div>
</template>

<script>


export default {
  props: ['timers', 'total', 'start'], 

  data() {
    return {
      addToTotal: null, 
    }
  },
  computed: {
     
  },
  methods: {
    startTime() {
      this.$emit('startTime', false); 
    },
    addValue(value,index) {
      this.timers[index].amount = Number(value.target.value);
      console.log(this.timers[index].amount);
    }
  },

}
</script>


<style>
 .setup {
  display: flex;
  display: flex;
  flex-direction: column;
  max-width: 700px;
  margin: 0 auto;
 }

 @media screen and (min-width: 1200px) {
   .setup { 
     width: 900px;
     max-width: none;
   }
 }

 .submit {
   background-color: orangered;
   border: none;
   color: #fff;
   font-size: 2.5rem;
   border-radius: 5px;
   outline: none;
   box-shadow: 1px 5px 3px rgba(0,0,0,.2);
   transition: box-shadow .2s ease;
   padding: 8px;
   width: 80px;
   cursor:pointer;
 }

 .submit:hover {
   box-shadow: 1px -2px 3px rgba(0, 0, 0, 0.1);
 }

  @media screen and (min-width: 1200px) {
   .submit {
     height: 4rem;
     width: 6rem;
   }
 }


 .setup-container {
   display: flex;
   flex-direction: column;
   justify-content: center;
   align-items: center;
   flex: 1 0 auto;
   margin-bottom: 2rem;
 }

 @media screen and (min-width: 1200px) {
   .setup-container {
     margin-bottom: 8rem;
   }
 }


 .setup__total {
   margin: 1rem auto;
   margin-bottom: 2rem;
   width: 60%;
   position: relative;
   max-width: 100%;
 }



.setup__phase {
  display: flex;
  padding: 1rem;
  align-items: center;
  justify-content: space-evenly;
}

@media screen and (max-width: 600px) {
   .setup__phase {
     padding: 1rem 0;
     margin: 0 5px;
   }
 }

.setup__display {
  margin-right: 5rem;
  width: 70%;
  max-width: 400px;
  position: relative;
}

 @media screen and (max-width: 600px) {
   .setup__display {
     margin-right: 2rem;
     width: 55%;
   }
 }


.setup__display span {
  position: absolute;
  left: 0;
  top: 0;
  width: 60%;
  height: 100%;
  text-align: center;
  line-height: 1.5;
  font-weight: bold;
  font-size: 2rem;
  background-color: orangered;
  border-top-left-radius: 5px;
  border: none;
  border-bottom-left-radius: 5px;
  color: #fff;
  text-transform: capitalize;
  box-shadow: inset -3px 0px 0px rgba(0, 0, 0, 0.1);
}

@media screen and (max-width: 600px) {
   .setup__display span {
     font-size: 1.3rem;
     width: 50%;
     line-height: 2.5;
   }
 }



.setup__total span {
  position: absolute;
  left: 0;
  top: 0;
  width: 60%;
  height: 100%;
  text-align: center;
  line-height: 1.5;
  font-weight: bold;
  font-size: 2rem;
  background-color: orangered;
  border-top-left-radius: 5px;
  border: none;
  border-bottom-left-radius: 5px;
  color: #fff;
  text-transform: capitalize;
  box-shadow: inset -3px 0px 0px rgba(0, 0, 0, 0.1);
}


.setup__input {
  width: 100%;
  height: 3rem;
  border-radius: 5px;
  border: 1px solid #ccc;
  text-align: right;
  outline: none;
  font-size: 2rem;
  padding-right: 10px;
  box-shadow: 1px 5px 10px rgba(0, 0, 0, 0.2);
  color: #757575;
}


 @media screen and (min-width: 1200px) {

   .setup__total span {
     line-height: 2;
     font-size: 3rem;
   }
   .setup__input { 
     height: 5rem;

   }
   .setup__display span {
     line-height: 2;
     font-size: 3rem; 
   }
 }



.adjusters {
  display: flex;
  width: 30%;
  justify-content: space-around;
}


@media screen and (max-width: 600px) {
   .adjusters {
     width: 40%;
   }
 }



.adjusters button {
  font-size: 3rem;
  border: none;
  text-align: center;
  padding: 0.5rem;
  outline: none;
  width: 3.5rem;
  height: 3.5rem;
  cursor: pointer;
  background-color: orangered;
  border-radius: 5px;
  color: #fff;
  display: block;
  line-height: 0;
  border: none;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.2);
}


 @media screen and (min-width: 1200px) {
   .adjusters button {
     height: 6rem;
     width: 6rem;
   }
 }

</style>

