<template>
  <div id="app">

    <h1>Vuemon</h1>

    <div id="simon">

      <div id="status">
        {{displayMessage}}
      </div>

      <div class="row">
        <div 
          id="green" 
          v-bind:class="{'bright':isActive && currentLight==='green'}" 
          class="light col" 
          v-on:click="captureTap('green')"> 
        </div>
        <div 
          id="red" 
          v-bind:class="{'bright':isActive && currentLight==='red'}" 
          class="light col" 
          v-on:click="captureTap('red')">
        </div>
      </div>

      <div class="row">
        <div 
          id="yellow" 
          v-bind:class="{'bright':isActive && currentLight==='yellow'}" 
          class="light col" 
          v-on:click="captureTap('yellow')">
        </div>
        <div 
          id="blue" 
          v-bind:class="{'bright':isActive && currentLight==='blue'}" 
          class="light col" 
          v-on:click="captureTap('blue')">
        </div>
      </div>

    </div>

    <div id="controls" class="row">
      <div class="col"><button class="start" v-on:click="start" >{{ buttonMessage }}</button></div>
    </div>

    <div id="history">
      <p><strong>Current Sequence:</strong> {{ current }}</p>
      <p><strong>Longest Sequence:</strong> {{ longest }}</p>
    </div>

  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      longest: 0,
      sequence: ['red','blank'],
      taps: '',
      lights: [ 'red', 'green', 'yellow', 'blue' ],
      isActive:false,
      currentLight:"",
      iterationCounter:0,
      displayMessage:'',
      buttonMessage:'Start'
    }
  },
  computed: {
    current: function() {
      return this.sequence.length/2;
    }
  },
  mounted() {
  },
  methods: {
    
    start: function() {
      this.sequence = [];
      this.addToSequence();
      this.playSequence(this.sequence);
      this.startTimer();
      this.iterationCounter=0;
      this.displayMessage='';

    },

    chooseRandomLight: function() {
      var index = Math.floor(Math.random() * 4);
      return this.lights[index];
    },

    addToSequence: function() { 
      this.sequence.push(this.chooseRandomLight(),'blank');
      this.buttonMessage="Restart";

    },

    playSequence: function(lightSelected) {
      // might be cool to increase speed after certain levels are achieved
      // Such as decrease time by 250ms after 10 sequences
      var self=this;

      lightSelected.forEach(function(el,index){
        setTimeout(function(){self.changeLightState(index);}, index*400);
        self.isActive=false;
      });
    },

    changeLightState: function(iteration){

      this.isActive=true;
      this.currentLight=self.sequence[iteration]; 
     
    },

    captureTap: function(color) {

      this.taps=color;
      this.currentLight=color;

      if(this.sequence[this.iterationCounter] === this.taps){
        this.iterationCounter+=2;

        if (this.iterationCounter===this.sequence.length){
          this.currentLight=false;
          this.addToSequence();
          this.playSequence(this.sequence);
          this.startTimer();
          this.iterationCounter=0;
        }

      }else{
        
        if(this.sequence.length/2 > this.longest){
          this.currentLight=false;
          this.longest=this.sequence.length/2;
          this.displayMessage="Sorry, not quite right. But hey, this is your longest sequence yet!";
          this.buttonMessage="Try Again";
        }
        else{
          this.currentLight=false;
          this.displayMessage="Sorry, not quite right. But you're close, give it another try.";
          this.buttonMessage="Try Again";
        }
      };

      console.log(this.taps);
    },

    startTimer: function() {
    }
  }
}
</script>

<style lang="scss">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  h1, h2 {
    font-weight: normal;
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
  #simon {
    padding: 20px;
  }
  #controls {
    padding-bottom: 20px;
  }
  #status {
    padding-bottom: 20px;
  }
  .row {
  }
  .col {
    display: inline-block;
  }
  .start {
    padding: 10px;
    font-size: 18px;
  }
  .light {
    margin: 20px;
    border: 1px solid #000;
  }
  #red {
    height: 100px;
    width: 100px;
    background: #E53A40;
    opacity: 0.1;
  }
  #yellow {
    height: 100px;
    width: 100px;
    background: #EFDC05;
    opacity: 0.1;
  }
  #green {
    height: 100px;
    width: 100px;
    background: #75D701;
    opacity: 0.1;
  }
  #blue {
    height: 100px;
    width: 100px;
    background: #30A9DE;
    opacity: 0.1;
  }
  .bright {
    opacity: 1.0 !important;
  }
</style>