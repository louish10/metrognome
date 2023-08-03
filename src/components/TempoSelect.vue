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
  <label class="tempo-value">Tempo: {{ tempo }} bpm</label>
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
  font-size: 24px;
}

.slider {
  accent-color: #39ff14;
}
</style>
