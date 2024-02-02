<script setup lang="ts">
defineProps<{
  onClick: Function;
}>();
</script>

<template>
  <button
    class="text-md rounded-md bg-red-200 p-1 shadow-md hover:opacity-80"
    @click="onClick()"
  >
    <slot />
  </button>
</template>
