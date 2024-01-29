<script setup lang="ts">
import { v4 } from 'uuid';

type TaskModel = {
  name: string;
  items: {
    name: string;
    amount: '' | number;
    unit: string;
  }[];
};

const formData = reactive<TaskModel>({
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
  <TaskForm 
    v-model="formData"
    :id="v4()"
  />
</template>
