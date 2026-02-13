<script setup>
const props = defineProps({
    total : Number,
    consistent : Number,
    building : Number,
    bestStreak : Number,
    streakProgress : Number,
    message : String
});
</script>

<template>
    <div class="status">
        <div class="stat">📋 Total: {{ total }}</div>
        <div class="stat">🔥 Consistent: {{ consistent }}</div>
        <div class="stat">🌱 Building: {{ building }}</div>
        <div class="stat">🏆 Best: 
            <span v-if="bestStreak">{{ bestStreak }} days</span>
            <span v-else>Start today 🚀</span>
        </div>
        <p class="message">{{ message }}</p>
    </div>
</template>

<style scoped>
.status{
    display: flex;
    flex-wrap: wrap;
    gap: 7px;
    margin: 12px 0;
    padding: 11px;
    border-radius: 9px;
    background: #eef2f2;
    color: var(--text-dark);
}

.stat{
    font-size: 17px;
    font-weight: 500;
}

.message{
    font-size: 20px;
    font-weight: 600;
    padding: 10px;
}
</style>