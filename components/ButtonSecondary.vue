<script setup lang="ts">
defineProps<{
  onClick: Function;
}>();
</script>

<template>
  <button class="
    bg-blue-200
    rounded-md
    p-1
    text-md
    shadow-md
    hover:opacity-80"
    @click="onClick()"
  >
    <slot />
  </button>
</template>
