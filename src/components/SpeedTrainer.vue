<script setup>
import { ref } from 'vue'
const emit = defineEmits(['tempoIncrement'])
const beatsPassed = ref(0)
const increaseBy = ref(4)
const increaseEvery = ref (1)

function incrementBeatsPassed() {

    beatsPassed.value += 1
    if (beatsPassed.value % increaseEvery.value == 0) {
        emit('tempoIncrement', increaseBy.value)
    }
}

function clearBeatsPassed() {
    beatsPassed.value = 0
}

defineExpose({
    incrementBeatsPassed,
    clearBeatsPassed
})

</script>

<template>
    <div class="sentence">
        <div>
            Increase tempo by 
        </div>
        <input type="number" v-model="increaseBy">
        <div>
            bpm every
        </div>
        <input type="number" v-model="increaseEvery">
        <div>
            beats.
        </div>
    </div>
</template>

<style scoped>
.sentence {
    display: flex;
    font-size: 30px;
}

input {
    margin: 0 20px;
    background-color: inherit;
    color: inherit;
    text-align: center;
    border-width: 0 0 2px 0;
    border-color: azure;
    font-size: inherit;
    width: 60px;
}

input[type=number] { 
    -moz-appearance: textfield;
    appearance: textfield;
}

input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button { 
      -webkit-appearance: none; 
}
</style>