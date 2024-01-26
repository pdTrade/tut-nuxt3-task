<script setup lang="ts">
const goAddPage = () => {
  navigateTo('/add');
};
</script>

<template>
  <BaseH1>やること管理</BaseH1>
  <BaseH2>一覧</BaseH2>

  <div class="text-right">
    <ButtonPrimary :onClick="goAddPage">タスクの追加</ButtonPrimary>
  </div>
</template>
