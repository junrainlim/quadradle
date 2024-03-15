<script setup lang="ts">
const props = defineProps(['index', 'value']);

const emit = defineEmits(['clickedCell']);

const styleObject = {
  borderRadius: '5px'
};

const clickListener = () => {
  emit('clickedCell', props.index);
};
</script>

<template>
  <div
    :id="'cell' + props.index"
    :class="'cell colour' + props.value"
    @click="clickListener"
    :style="styleObject"
  ></div>
</template>
