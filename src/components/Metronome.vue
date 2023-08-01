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

function toggleRunning() {
  running.value = !running.value
}

function changeTempo(newTempo) {
  tempo.value = newTempo
}
</script>

<template>
  <div>
    <h1>Metrognome {{ tempo }}</h1>
  </div>
  <div>{{ running }}</div>
  <button @click="toggleRunning">{{ running ? 'stop' : 'start' }}</button>
  <TempoSelect @tempo-change="changeTempo"></TempoSelect>
</template>

<style scoped></style>
