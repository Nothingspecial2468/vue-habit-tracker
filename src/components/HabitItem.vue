<script setup>
const props = defineProps({
    habit : Object
})

const emit = defineEmits(['delete', 'update-streak'])
</script>

<template>
    <li class="item">
        <input type="checkbox"
        v-model="habit.done"
        @change="emit('update-streak', habit)"
        >

        <span :class="{done: habit.done}">
            {{ habit.title }}
        </span>

        <span class="streak">
            🔥 {{ habit.streak }}
        </span>

        <button
        class="delete"
        @click="emit('delete',habit.id)">
        ❌
        </button>
    </li>
</template>

<style scoped>
</style>