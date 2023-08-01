<script setup>
import { ref } from 'vue'
const tempo = ref(60)
defineEmits(['tempoChange'])
</script>

<template>
  <input v-on:change="$emit('tempoChange', 60)" type="radio" id="60" value="60" v-model="tempo" />
  <label for="60">60</label>
  <input v-on:change="$emit('tempoChange', 80)" type="radio" id="80" value="80" v-model="tempo" />
  <label for="80">80</label>
</template>
