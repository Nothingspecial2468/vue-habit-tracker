<script setup>
import {ref} from 'vue'

const emit = defineEmits(['add']);

const habit = ref('');
const warningMsge = ref('');

function submit(){
    const value = habit.value.trim();

    if(!value){
        warningMsge.value = 'Please enter a habit first!';
        return;
    }
    
    emit('add', value);

    habit.value='';
    warningMsge.value='';

}
</script>

<template>
    <div class="input-section">
        <input v-model="habit"
        @keyup.enter = "submit"
        @input="warningMsge = ''"
        placeholder="Enter your habit here....">

        <p v-if="warningMsge" class="warning">{{ warningMsge }}</p>

        <button @click="submit">Add Habit</button>
    </div>
</template>

<style scoped>
.input-section{
    padding: 4px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

input{
    border: 1px solid black;
    border-radius: 6px;
    font-size: 18px;
    font-weight: 500;
    padding:8px 10px;
    gap: 8px;
}

.warning{
    color: rgb(157, 35, 35);
    font-size: 17px;
}

button{
    margin: 8px;
    border: 1px solid #01010b;
    font-size: 15px;
    font-weight: 600;
    border-radius: 6px;
    padding: 10px 16px;
}
</style>