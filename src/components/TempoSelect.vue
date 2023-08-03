<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'
const tempo = ref(60)
const emit = defineEmits(['tempoChange'])

onMounted(() => {
  window.addEventListener('keydown', (e) => {
    switch (e.key) {
      case 'ArrowLeft':
        tempo.value -= 4
        emit('tempoChange', tempo.value)
        break
      case 'ArrowRight':
        tempo.value += 4
        emit('tempoChange', tempo.value)
        break
    }
  })
})
</script>

<template>
  <label class="tempo-value">Tempo: {{ tempo }} bpm</label>
  <input
    class="slider"
    v-model="tempo"
    v-on:change="$emit('tempoChange', tempo)"
    type="range"
    min="20"
    max="300"
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
