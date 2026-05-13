<script setup lang="ts">
const props = defineProps(['index']);

const emit = defineEmits(['clickedPalette', 'hoveredPalette', 'unhoveredPalette']);

const clickListener = () => {
  emit('clickedPalette', props.index);
};
</script>

<template>
  <div
    :id="'circle' + props.index"
    :class="'circle colour' + props.index"
    @click="clickListener"
    @mouseenter="emit('hoveredPalette', props.index)"
    @mouseleave="emit('unhoveredPalette')"
    :style="{ borderRadius: '50%' }"
  ></div>
</template>
