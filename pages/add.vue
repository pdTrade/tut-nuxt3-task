<script setup lang="ts">

const formData = reactive({
  name: '',
  items: [
    {
      name: '',
      amount: '',
      unit: '',
    },
  ],
});
</script>

<template>
  <BaseH1>やること管理</BaseH1>
  <BaseH2>タスク追加</BaseH2>
  <TaskForm v-model="formData"/>
</template>
