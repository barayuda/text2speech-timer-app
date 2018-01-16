<template>
  <div class="ui segment" id="timer-container" v-cloak>
    <h1 class="ui header">Timer</h1>
    <div class="ui relaxed divided selection list">
    	<timer-setup v-if="!time" @set-time="setTime"></timer-setup>
    	 <div v-else>
    		 <timer :time="prettyTime"></timer>
          <div class="ui buttons">
            <button class="ui positive button" v-if="!isRunning" @click="start">Start</button>
            <button class="ui button" @click="reset">Reset</button>
            <button class="ui button" v-if="isRunning" @click="stop">Stop</button>
          </div>
    	</div>
    </div>
  </div>
</template>

<script>
  import TimerSetup from './TimerSetup.vue'
  import Timer from './Timer.vue'
  import Artyom from 'artyom.js'

  const Bara = new Artyom();

  export default {

    data () {
      return {
        isRunning: false,
        minutes:0,
        secondes:0,
        time:0,
        timer:null,
        // Text-to-speech will be here later
        sound:new Audio("http://s1download-universal-soundbank.com/wav/nudge.wav")
        // sound: Bara.say('Time-up!')
      }
    },

    computed: {
  		prettyTime () {
  			 let time = this.time / 60
  			 let minutes = parseInt(time)
  			 let secondes = Math.round((time - minutes) * 60)
  			 return minutes+":"+secondes
  		}
  	},
  	methods: {
  		 start () {
  			 this.isRunning = true
  			 if (!this.timer) {
  				  this.timer = setInterval( () => {
  						if (this.time > 0) {
  							 this.time--
  						} else {
  							 clearInterval(this.timer)
  							 this.sound.play()
  							 this.reset()
  						}
  				  }, 1000 )
  			 }
  		 },
  		 stop () {
  			 this.isRunning = false
  			 clearInterval(this.timer)
  			 this.timer = null
  		 },
  		 reset () {
  			  this.stop()
  			  this.time = 0
  			  this.secondes = 0
  			  this.minutes = 0
  		 },
  		 setTime (payload) {
  			 this.time = (payload.minutes * 60 + payload.secondes)
  		 }
  	},

    components: {
      TimerSetup,
      Timer
    },

  }

</script>
