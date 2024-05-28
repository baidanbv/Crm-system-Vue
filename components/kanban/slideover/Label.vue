<script setup lang="ts">
const props = defineProps({
  labaleText: {
    type: String,
    default: ''
  }
});
</script>

<template>
  <div class="mb-3">
    <label v-if="labaleText" class="opacity-75 mb-0.5 block text-xs">{{ labaleText }}</label>
    <div class="text-base"><slot /></div>
  </div>
</template>

