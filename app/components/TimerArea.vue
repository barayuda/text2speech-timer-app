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
          {{ custom_artyom }}
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
        custom_artyom: "",
        // sound:new Audio("http://s1download-universal-soundbank.com/wav/nudge.wav")
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
  							 // this.sound.play()
                 Bara.initialize({
                   lang: "id-ID",
                   debug: true, // Show messages in the console
                   // If providen, you can only trigger a command if you say its name
                   // e.g to trigger Good Morning, you need to say "Jarvis Good Morning"
                   name: "Bara"
                 }).then(() => {
                   console.log('%c [GET VOICE] ' + '%c' + Bara.getVoices(), 'background: #000; color: #0F0', 'color: #000');
                   console.log('%c [Initialize] ' + '%c Artyom has been succesfully initialized', 'background: #000; color: #0F0', 'color: #000');
                 }).catch((err) => {
                   console.error("Artyom couldn't be initialized: ", err);
                 });

                 Bara.say(this.custom_artyom,{
                      onStart:function(){
                        console.log('%c [onStart] ' + '%c The text is being readed', 'background: #000; color: #fff', 'color: #000');
                      },
                      onEnd:function(){
                        console.log('%c [onEnd] ' + '%c Well, that was all!', 'background: #000; color: #fff', 'color: #000');
                      }
                  })

                  Bara.fatality();
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
         this.custom_artyom = payload.custom_artyom
  		 }
  	},

    components: {
      TimerSetup,
      Timer
    },

  }

</script>
