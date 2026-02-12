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
.item{
    display: flex;
    align-items: center;
    justify-content:space-evenly;
    gap: 9px;
    padding: 10px;
    background: var(--primary-dark);
    color: var(--primary-light);
    border-radius: 6px;
}

.item:hover{
    transform: translateY(-2px);
}

.item span{
    flex: 1;
    text-align: left;
}

.streak{
    opacity: 0.7;
    font-size: 0.9rem;
}

.done{
    text-decoration: line-through;
    color: var(--primary-light);
    opacity: 0.5;
}

.delete{
    cursor: pointer;
    background: transparent;
    border: none;
    font-size: 0.7rem;
    transition: transform 0.2s ease-in-out;
}

.delete:hover{
    transform: scale(1.2);
}

</style>