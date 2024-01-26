<script setup lang="ts">
const props = defineProps<{ modelValue: string }>();
const modelValue = computed({
  get: () => props.modelValue,
})
</script>

<template>
  <textarea
    v-model="modelValue"
    type="number"
    class="h-30 w-full rounded-lg border-2 p-2"
  />
</template>
