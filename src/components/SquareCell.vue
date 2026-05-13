<script setup lang="ts">
const props = defineProps(['index', 'value', 'isOriginal', 'isHighlighted']);

const emit = defineEmits(['clickedCell']);

const clickListener = () => {
  emit('clickedCell', props.index);
};
</script>

<template>
  <div
    :id="'cell' + props.index"
    :class="['cell', 'colour' + props.value, { 'cell-original': props.isOriginal, 'cell-highlighted': props.isHighlighted }]"
    @click="clickListener"
    :style="{ borderRadius: '5px' }"
  ></div>
</template>
