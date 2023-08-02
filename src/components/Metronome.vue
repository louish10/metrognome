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
const audio = new Audio('../../public/click.wav')

function clickInterval() {
  // tempo is in bpm. Want to convert to interval in ms
  return (60 * 1000) / tempo.value
}

function playAudio() {
  audio.play()
  intervalId.value = setInterval(() => {
    audio.play()
  }, clickInterval())
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

  if (running.value) {
    clearInterval(intervalId.value)
    playAudio()
  }
}
</script>

<template>
  <div class="contents">
    <div>
      <h1 class="header">Metrognome</h1>
    </div>
    <TempoSelect @tempo-change="changeTempo"></TempoSelect>
    <button class="start-button" @click="toggleRunning">{{ running ? 'stop' : 'start' }}</button>
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
  border-radius: 10px;
  color: #39ff14;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 24px;
}
</style>
