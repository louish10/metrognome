<script setup>
import { ref } from 'vue'
const emit = defineEmits(['tempoIncrement'])
const beatsPassed = ref(0)
const increaseBy = ref(4)
const increaseEvery = ref (16)

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
            beats. {{beatsPassed}}
        </div>
    </div>
</template>

<style scoped>
.sentence {
    display: flex
}
</style>