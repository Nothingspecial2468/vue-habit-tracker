<script setup>
import { ref , computed , onMounted , watch} from 'vue';
import HabitItem from './components/HabitItem.vue';
import HabitStats from './components/HabitStats.vue';

const title = ref('Habit Tracker');
const habit = ref('')
const warningMsge = ref('');

const habitList = ref([
]);

const totalHabits = computed(()=>
    habitList.value.length
);

const consistentHabits = computed(()=>{
    return habitList.value.filter(h=> h.streak >=3).length
});

const buildingHabits = computed(()=>{
    return habitList.value.filter(h=> h.streak < 3).length
});

const bestStreak = computed(()=>{
    return habitList.value.length ? Math.max(...habitList.value.map(h=> h.streak))
    : 0
});

const STREAK_GOAL = 7;

const streakProgress = computed(()=>{
    return Math.min((bestStreak.value / STREAK_GOAL) *100, 100);
});

const computedMessage = computed(()=>{
    if(totalHabits.value === 0 ){
        return "Let's start with small things.";
    }

    if(consistentHabits.value >0){
        return "🔥 You’re building real consistency. Keep going!";
    }

    return "🌱 Small steps daily. Your streak begins now.";
});

const saveStatus = ref('Saved ✓');
const showStatus = ref(false);

const addHabit =()=>{
    const value = habit.value.trim();

    if(!value){
        warningMsge.value = "Please enter a habit first!";
        return;
    }

    warningMsge.value = "";

    habitList.value.push({
        id: Date.now(),
        title: value,
        done: false,
        lastCompleted : null,
        streak : 0
    });

    habit.value = "";
}


function deleteHabit(id) {
    return habitList.value = habitList.value.filter(h => h.id !== id);
}

function updateStreak(habit){
    if(!habit.done) return;

    const today = new Date().toDateString();
    const last = habit.lastCompleted;

    if(!last){
        habit.streak = 1;
    }
    else{
        const diff = 
        (new Date(today) - new Date(last)) / (1000*60*60*24);

        if(diff === 1){
            habit.streak += 1
        }
        else if(diff >1){
            habit.streak =1
        }
    }
    habit.lastCompleted = today;
}

onMounted(()=>{
    const saved = localStorage.getItem('habits');

    if(saved){
        habitList.value = JSON.parse(saved);
    }
})

watch(habitList , ()=>{
    showStatus.value=true;
    saveStatus.value= 'Saving....';
    
    setTimeout(() => {
        localStorage.setItem('habits', JSON.stringify(habitList.value));
        saveStatus.value = 'Saved ✓'
    },300);

    setTimeout(() => {
        showStatus.value = false
    }, 1400);

}, {deep:true})

</script>

<template>
  <div class="app-container">
    <div class="app-card">
        <h1>{{ title }}</h1>
        <input v-model="habit" 
        @input="warningMsge = ''"
        @keyup.enter = "addHabit"
        type="text" placeholder="Enter your habit here....">
    
        <p v-if="warningMsge" class="warning">
            {{ warningMsge }}
        </p>

        <button @click="addHabit">Add Habit</button>

        <p v-if="!habitList.length" class="empty">
            No habits yet. Start with one ✨
        </p>

        <h3>Your habit list:</h3>
        
        <HabitStats
        :total="totalHabits"
        :consistent="consistentHabits"
        :building="buildingHabits"
        :best-streak="bestStreak"
        :message="computedMessage">
        </HabitStats>

        <div class="streak-bar">
            <div class="streak-fill"
            :style="{width: streakProgress + '%'}">
            </div>
        </div>

        <div class="streak-text">
            {{ bestStreak }} / 7 days
        </div>

        <div class="save-dropdown" v-if="showStatus">
            {{ saveStatus }}
        </div>

        <ul>
            <HabitItem
            v-for="value in habitList"
            :key="value.id"
            :habit="value"
            @delete="deleteHabit"
            @update-streak="updateStreak"
            ></HabitItem>
        </ul>
    </div>
  </div>
</template>

<style scoped>
/* 1. Define a color palette with CSS Variables */
.app-container {
    --primary-dark: #0e173f;
    --primary-light: whitesmoke;
    --accent-color: #4a90e2; /* A slightly brighter blue for interaction */
    --text-light: #ffffff;
    --text-dark: #0e173f;
    --shadow-color: rgba(0, 0, 0, 0.2);
    --border-color: #ccc;
    --danger-color: #e74c3c; /* For potential delete buttons */

    background: var(--primary-dark);
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
    min-height: 100vh;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}

.app-card {
    width: 360px;
    background: var(--primary-light);
    padding: 2rem; 
    border-radius: 12px;
    text-align: center;
    box-shadow: 0 8px 24px var(--shadow-color);
    display: flex;
    flex-direction: column;
    gap: 1rem; 
    position: relative;
}

h1 {
    margin-bottom: 1.5rem;
    color: var(--text-dark);
    font-weight: 700;
}

input[type="text"] {
    padding: 0.75rem;
    border-radius: 8px;
    border: 1px solid var(--border-color);
    font-size: 1.1rem;
    transition: border-color 0.3s, box-shadow 0.3s; 
}


input[type="text"]:focus-visible {
    outline: none;
    border-color: var(--accent-color);
    box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.3);
}

button {
    padding: 0.75rem;
    border-radius: 8px;
    cursor: pointer;
    border: none;
    background: var(--primary-dark);
    color: var(--text-light);
    font-size: 1.1rem;
    font-weight: 600;
    margin-bottom: 1rem;
    transition: background-color 0.3s, transform 0.2s; 
}

button:hover {
    background-color: #1c2a5e; 
    transform: translateY(-2px);
}

button:focus-visible {
    outline: none;
    box-shadow: 0 0 0 3px rgba(74, 144, 226, 0.3);
}

.warning{
    color: #e74c3c;
  font-size: 0.9rem;
  margin: 6px 0;

}

p {
    font-size: 1.1rem;
    color: var(--text-dark);
}

ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

.status{
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 10px;
    justify-content: center;
}

.stat{
    background: #e6ecff;
    padding: 6px 10px;
    border-radius: 7px;
    font-size: 15px;
    font-weight: 600;
}

.message{
    font-size: 20px;
    font-weight: 600;
    color:#1c2a5e;
    margin-bottom: 10px;
}

.steak-bar{
    width: 100%;
    height: 10px;
    background: #b9c3c9;
    border-radius: 10px;
    overflow: hidden;
    margin: 8px 0;
}

.streak-fill{
    height: 100%;
    background: #4a90e2;
    transition: width 0.3s ease-in-out;
}

.streak-text{
    font-size: 14px;
    color: var(--text-dark);
    font-weight: 600;
}

.item {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 12px 16px;
    margin: 0.5rem auto;
    background: var(--primary-dark);
    color: var(--text-light);
    border-radius: 8px;
    font-size: 1rem;
    transition: transform 0.3s ease-in-out;
}

.item:hover {
    transform: scale(1.03); 
}

.item input[type="checkbox"] {
    transform: scale(1.2);
    cursor: pointer;
    accent-color: var(--accent-color); 
}

.item span{
    flex: 1;
    text-align: left;
}

.streak{
    font-size: 15px;
    opacity: 0.7;
}

.save-dropdown{
    position: absolute;
    top: 10px;
    right: 10px;

    background: white;
    color: #0e173f;

    padding: 6px 12px;
    border-radius: 5px;

    font-size: 15px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.6);

    animation: slideDown 0.25s ease;
}

@keyframes slideDown{
    from{
        opacity: 0;
        transform: translateY(-10px);
    }
    to{
        opacity: 1;
        transform: translateY(0);
    }
}

.delete{
    /* margin-left: auto; */
    padding-left: 10px;
    cursor: pointer;
    border: none;
    font-size: 16px;
    background: transparent;
    transition: transform 0.14s ease;
}

.delete:hover{
    transform: scale(1.2);
}

.empty {
    color: var(--text-dark);
    font-size: 1.1rem;
    margin-bottom: 0.75rem;
}

.done {
    text-decoration: line-through;
    opacity: 0.6;
}
</style>