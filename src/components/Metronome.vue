<script setup>
import { ref } from 'vue'
import TempoSelect from './TempoSelect.vue'
import Button from './Button.vue'
import SpeedTrainer from './SpeedTrainer.vue'

defineProps({
  msg: {
    type: String,
    required: true
  }
})

const speedTrainer = ref(null)
const tempoSelect = ref(null)

const running = ref(false)
const tempo = ref(60)
const intervalId = ref(0)
const modes = ['Normal', 'Speed trainer']
const mode = ref(0)

let context = null
let audioBuffer = null

let lastClick = 0
let nextClick = 0
let clickScheduled = false

function clickInterval() {
  // tempo is in bpm. Want to convert to interval in s
  return 60 / tempo.value
}

function scheduleClick(time) {
  let source = context.createBufferSource()
  source.buffer = audioBuffer
  source.connect(context.destination)
  source.start(time)
  clickScheduled = true
}

async function playAudio() {
  if (!context) {
    context = new AudioContext()
    audioBuffer = await fetch('click.wav')
      .then((res) => res.arrayBuffer())
      .then((ArrayBuffer) => context.decodeAudioData(ArrayBuffer))
  }

  nextClick = context.currentTime
  scheduleClick(nextClick)

  intervalId.value = setInterval(() => {
    if (context.currentTime > nextClick) {
      clickScheduled = false
      lastClick = nextClick
      nextClick = lastClick + clickInterval()
      if (speedTrainer.value) {
        speedTrainer.value.incrementBeatsPassed()
      }
    }
    if (!clickScheduled && nextClick - context.currentTime < 0.1) {
      scheduleClick(nextClick)
    }
  }, 50)
}

function stopAudio() {
  clearInterval(intervalId.value)
  if(speedTrainer.value) {
    speedTrainer.value.clearBeatsPassed()
  }
}

function toggleRunning() {
  running.value = !running.value

  if (running.value) {
    playAudio()
  } else {
    stopAudio()
  }
}

function changeTempo(newTempo) {
  if (newTempo >= 20 && newTempo <= 300){
    tempo.value = newTempo
  }
}

function incrementTempo(tempoIncrement) {
  changeTempo(tempo.value + tempoIncrement)
  tempoSelect.value.setTempo(tempo.value)

}
</script>

<template>
  <div class="contents">
    <div>
      <h1 class="header">Metrognome</h1>
    </div>
    <TempoSelect ref="tempoSelect" @tempo-change="changeTempo"></TempoSelect>
    <button class="start-button" @click="toggleRunning">{{ running ? 'Stop' : 'Start' }}</button>
    <SpeedTrainer v-if="mode" ref="speedTrainer" @tempo-increment="incrementTempo"></SpeedTrainer>
    <div class="radio-container">
      <input class="radio-input" type="radio" id="normal" :value="0" v-model="mode" />
      <label class="radio-input-label" for="normal">{{ modes[0] }}</label>
      <input class="radio-input" type="radio" id="speed-trainer" :value="1" v-model="mode" />
      <label class="radio-input-label" for="speed-trainer">{{ modes[1] }}</label>
    </div>
  </div>
</template>

<style scoped>
.header {
  text-align: center;
  font-size: 70px;
}

.contents {
  display: flex;
  flex-direction: column;
  max-width: 900px;
  margin: auto;
}

.start-button {
  background-color: transparent;
  border: 2px solid #39ff14;
  border-radius: 16px;
  color: #39ff14;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 48px;
  margin: 10px 0px;
}

.radio-container {
  margin: 20px 0px;
  display: flex;
  justify-content: center;
  gap: 10px;
}
.radio-input {
  display: none;
}

.radio-input-label {
  border: 2px solid #39ff14;
  font-size: 48px;
  padding: 15px 32px;
  margin: 10px 0px;
  border-radius: 15px;
}

.radio-input:checked + .radio-input-label {
  background-color: #39ff14;
  color: #404040;
}
</style>
