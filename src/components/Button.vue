<script setup>
const props = defineProps(['label', 'cb'])
</script>

<template>
  <button class="button" @click="cb">{{ props.label }}</button>
</template>

<style scoped>
.button {
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

.button:hover {
  background-color: #39ff1411
}

.button:active {
  background-color: #39ff1422
}
</style>
