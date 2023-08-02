<script setup>
import { ref } from 'vue'
const tempo = ref(60)
defineEmits(['tempoChange'])
</script>

<template>
  <label class="tempo-value">Tempo: {{ tempo }} bpm</label>
  <input
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
</style>
