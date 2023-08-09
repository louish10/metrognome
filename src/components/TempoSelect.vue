<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'
const tempo = ref(60)
const emit = defineEmits(['tempoChange'])
const minTempo = 20
const maxTempo = 300

onMounted(() => {
  window.addEventListener('keydown', (e) => {
    switch (e.key) {
      case 'ArrowLeft':
        decrementTempo()
        emit('tempoChange', tempo.value)
        break
      case 'ArrowRight':
        incrementTempo()
        emit('tempoChange', tempo.value)
        break
    }
  })
})

function decrementTempo() {
  tempo.value = Math.max((tempo.value -= 4), minTempo)
}

function incrementTempo() {
  tempo.value = Math.min((tempo.value += 4), maxTempo)
}
</script>

<template>
  <label class="tempo-value">{{ tempo }} bpm</label>
  <input
    class="slider"
    v-model="tempo"
    v-on:change="$emit('tempoChange', tempo)"
    type="range"
    v-bind:min="minTempo"
    v-bind:max="maxTempo"
  />
</template>

<style scoped>
.tempo-value {
  font-size: 48px;
  text-align: center;
}

.slider {
  accent-color: #39ff14;
  -webkit-appearance: none;
  appearance: none;
  width: max;
  height: 40px;
  margin: 20px 0px;
  background-color: inherit;
  border: 5px solid #39ff14;
  border-radius: 15px;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 40px;
  height: 40px;
  background-color: #39ff14;
  border: none;
  border-radius: 0px;
}

.slider::-moz-range-thumb {
  appearance: none;
  width: 40px;
  height: 40px;
  background-color: #39ff14;
  border: none;
  border-radius: 0px;
}
</style>
