<script setup>
const props = defineProps({
    total:Number,
    consistent: Number,
    building: Number
});

const percent = props.total ? Math.round((props.consistent / props.total) *100) : 0;
</script>

<template>
    <div class="streak-wrapper">
        <div class="label">
            Consistency {{ percent }}%
        </div>

        <div class="bar">
            <div class="fill"
            :style="{width: percent + '%'}">
            </div>
        </div>
    </div>
</template>

<style scoped>
.streak-wrapper {
  margin: 1rem 0;
}

.label {
  font-size: 0.9rem;
  margin-bottom: 6px;
}

.bar {
  height: 10px;
  background: #ddd;
  border-radius: 10px;
  overflow: hidden;
}

.fill {
  height: 100%;
  background: #4a90e2;
  transition: width 0.4s ease;
}
</style>