<script setup>
import { ref } from 'vue'
import TempoSelect from './TempoSelect.vue'

defineProps({
  msg: {
    type: String,
    required: true
  }
})

const running = ref(false)
const tempo = ref(60)
const intervalId = ref(0)
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
    }
    if (!clickScheduled && nextClick - context.currentTime < 0.1) {
      scheduleClick(nextClick)
    }
  }, 50)
}

function stopAudio() {
  clearInterval(intervalId.value)
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
  tempo.value = newTempo
}
</script>

<template>
  <div class="contents">
    <div>
      <h1 class="header">Metrognome</h1>
    </div>
    <TempoSelect @tempo-change="changeTempo"></TempoSelect>
    <button class="start-button" @click="toggleRunning">{{ running ? 'Stop' : 'Start' }}</button>
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
}
</style>
