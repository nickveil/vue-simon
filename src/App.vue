<template>
  <div id="app">

    <h1>OKLAHOMA THEMED</h1>
    <h2> Simon Clone (Vue.js)</h2>
    <div id="status">
      {{displayMessage}}
    </div>    
    <div id="simon">
      <div class="row">
        <div 
          id="green" 
          v-bind:class="{'bright':isActive && currentLight==='green'}" 
          class="light col noSelect" 
          v-on:click="captureTap('green')"> 
        </div>
        <div 
          id="red" 
          v-bind:class="{'bright':isActive && currentLight==='red'}" 
          class="light col noSelect" 
          v-on:click="captureTap('red')">
        </div>
      </div>

      <div class="row">
        <div 
          id="yellow" 
          v-bind:class="{'bright':isActive && currentLight==='yellow'}" 
          class="light col noSelect" 
          v-on:click="captureTap('yellow')">
        </div>
        <div 
          id="blue" 
          v-bind:class="{'bright':isActive && currentLight==='blue'}" 
          class="light col noSelect" 
          v-on:click="captureTap('blue')">
        </div>
      </div>
    </div>

    <div id="controls" class="row">
      <div class="col"><button class="start mt-5" v-on:click="start" >{{ buttonMessage }}</button></div>
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
      buttonMessage:'Start',
      disabled:true,
      speed:400

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
      this.speed=400;
      

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
      clearTimeout();
      if(this.sequence.length/2 > 4){
        this.speed=350;
        if(this.sequence.length/2 > 9){
          this.speed=300;
          if(this.sequence.length/2 > 14){
            this.speed =250;
            if(this.sequence.length/2 > 19){
              this.speed =200;
            }
          }
        }
      }

      var self=this; 
      lightSelected.forEach(function(el,index){
        setTimeout(function(){
          self.changeLightState(index); }, index*self.speed); self.isActive=false; });
    },

    changeLightState: function(iteration){
      var self=this;

      this.isActive=true;
      this.currentLight=self.sequence[iteration]; 
     
    },
    captureTap: function(color,time) {

      this.taps=color;
      this.currentLight=color;
      //console.log("captureTap_currentLight: ",this.currentLight)
      setTimeout(function (){this.gameEvaluation(color)}.bind(this),200);
      },

    gameEvaluation:function(color){

      


      if(this.sequence[this.iterationCounter]=== this.taps){
        this.iterationCounter+=2;
        this.currentLight="blank";
        //console.log("gameEvaluation_currentLight: ",this.currentLight);
        clearTimeout();

        if (this.iterationCounter===this.sequence.length){
          this.currentLight=false;
          this.addToSequence();
          setTimeout(function(){this.playSequence(this.sequence)}.bind(this),2000);
          this.startTimer();
          this.iterationCounter=0;
        }
      }
      else{
        
        if(this.sequence.length/2 > this.longest){
          this.currentLight=false;
          this.longest=(this.sequence.length/2)-1;
          this.displayMessage="Sorry, not quite right. But hey, this is your longest sequence yet!";
          this.buttonMessage="Try Again";
          clearTimeout();
        }else{
          this.currentLight=false;
          this.displayMessage="Sorry, not quite right. But you're close, give it another try.";
          this.buttonMessage="Try Again";
        }
      };
    },

    startTimer: function() {
    }
  }
}

</script>

<style lang="scss">
@font-face{
  font-family: jerseyLetters;
  src:url("/College Player_demo.otf");
}
body {
  background-image: url("assets/Tight_ou_bkg.jpg");
  background-size: cover;
  background-repeat: no-repeat;

}

#simon{
  background-color: white;
  margin-bottom: 20px;
  max-width: 480px;
  margin:15px auto;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  margin-top: 60px;
}
h1 {
  margin-bottom: 5px;
  color:white;
  font-size: 80px;
  font-family: jerseyLetters;
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
  color: white;
}
#simon {
  padding: 20px;
}
#controls {
  padding-bottom: 20px;
}
#status {
  padding-bottom: 20px;
  height: 20px;
  font-size: 25px;
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
  // border: 1px solid #000;
}
#red { // changed to helmet
  height: 150px;
  width: 170px;
  background-image: url("assets/okla-helmet-right.png");
  background-repeat:no-repeat;
  background-size:170px 150px;
  opacity: 0.1;
}
#yellow {
  height: 150px;
  width: 170px;
  background-image: url("assets/helmet_left.jpg");
  background-repeat:no-repeat;
  background-size:170px 150px;
  opacity: 0.1;
}
#green { // changed to
  height: 150px;
  width: 170px;
  background-image: url("assets/gloves-800.jpg");
  background-repeat:no-repeat;
  background-size:170px 150px;
  opacity: 0.1;
}
#blue {
  height: 150px;
  width: 170px;
  background-image: url("assets/oklahoma_sooners.png");
  background-repeat:no-repeat;
  background-size:170px 150px;
  opacity: 0.1;
}
.bright {
  opacity: 1.0 !important;
}
.noSelect {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

</style>