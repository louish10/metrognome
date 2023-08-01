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
  <div>
    <h1>Metrognome</h1>
  </div>
  <div>{{ running }}</div>
  <button @click="toggleRunning">{{ running ? 'stop' : 'start' }}</button>
  <TempoSelect @tempo-change="changeTempo"></TempoSelect>
</template>

<style scoped></style>
