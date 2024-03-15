<script setup lang="ts">
const props = defineProps(['index']);

const emit = defineEmits(['clickedPalette']);

const styleObject = {
  borderRadius: '50%'
};

const clickListener = () => {
  emit('clickedPalette', props.index);
};
</script>

<template>
  <div
    :id="'circle' + props.index"
    :class="'circle colour' + props.index"
    @click="clickListener"
    :style="styleObject"
  ></div>
</template>
