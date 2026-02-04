<script setup>
import { ref } from 'vue';
const title = ref('Habit Tracker');
const habit = ref('')

const habitList = ref([
]);

const addHabit =()=>{
    const value = habit.value.trim();

    if(!value) return

    habitList.value.push({
        id: Date.now(),
        title: value,
        done: false
    });

    habit.value = "";
}

</script>

<template>
  <div class="app-container">
    <div class="app-card">
        <h1>{{ title }}</h1>
        <input v-model="habit" 
        @keyup.enter = "addHabit"
        type="text" placeholder="Enter your habit here....">
    
        <button @click="addHabit">Add Habit</button>

        <p v-if="!habitList.length" class="empty">
            No habits yet. Start with one ✨
        </p>

        <ul>
            <p>Your habit list:</p>
            <li v-for="value in habitList" :key="value.id" class="item">
            {{ value.title }}
            </li>
        </ul>
    </div>
  </div>
</template>

<style scoped>
.app-container{
    background: #0e173f;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    min-height: 50vh;
}

.app-card{
    width: 360px;
    background: whitesmoke;
    padding: 30px;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
}

h1{
    margin-bottom: 25px;
    color: #0e173f;
}

input{
    width: 100%;
    padding:12px;
    border-radius: 9px;
    border: 1px solid rgb(11, 7, 40);
    margin-bottom: 13px;
    font-size: 20px;
    text-align: center;
}

button{
    width: 100%;
    padding: 11px;
    border-radius: 8px;
    cursor: pointer;
    border: none;
    background: rgb(15, 35, 72);
    color: white;
    font-size: 20px;
    font-weight: 600;
    margin-bottom: 15px;
}

button:hover{
    opacity: 0.9;
}

p{
    font-size: 20px;
    color: #0e173f;
}

ul{
    list-style: none;
    margin: 0;
    padding: 0;
}

.item{
    padding: 11px;
    margin-bottom: 8px;
    background: #0e173f;
    color: white;
    border-radius: 8px;
    font-size: 22px;
}

.empty{
    color: #0e173f;
    font-size: 20px;
    margin-bottom: 11px;
}
</style>
