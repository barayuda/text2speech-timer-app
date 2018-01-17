<template>
  <form>
    <div class="ui tiny form segment">
      <div class="two fields">
        <div class="field">
          <label>Text</label>
          <input type="text" v-model="custom_artyom" name="custom_artyom" id="custom_artyom">
        </div>
        <div class="field">
          <label>Menit</label>
          <input type="number" v-model="minutes" name="time_m" id="min" min="0" max="59">
        </div>
        <div class="field">
          <label>Detik</label>
          <input type="number" v-model="secondes" name="time_s" id="sec" max="59" min="0">
        </div>
      </div>
      <button class="ui submit button" type="button" @click="sendTime">Set time</button>
    </div>
  </form>
</template>

<script>
  import { shell } from 'electron'

  export default {
    data () {
		 return {
        custom_artyom: "",
			  minutes:0,
			  secondes:0
		 }
  	},
  	methods: {
		 sendTime() {
		  this.$emit('set-time', {custom_artyom: this.custom_artyom, minutes:this.minutes, secondes:this.secondes})
		 }
  	}

  }
</script>
