<script setup lang="ts">
const dbName = 'taskDB';
const storeName = 'taskStore';

const links = ref<{
  url: string;
  text: string;
}[]>([]);;

onMounted(() => {
  const openReq = indexedDB.open(dbName);

  openReq.onupgradeneeded = (event) => {
    const db = event.target.result;
    db.createObjectStore(storeName, { keyPath: 'id' });
  };

  openReq.onsuccess = (event) => {
    const db = event.target.result;
    const trans = db.transaction(storeName, 'readonly');
    const store = trans.objectStore(storeName);

    const cursorReq = store.openCursor();

    cursorReq.onsuccess = (event) => {
      const cursor = event.target.result;

      if (!cursor) return;

      const row = cursor.value;

      links.value.push({
        url: `${row.id}`,
        text: row.name
      });

      cursor.continue();
    };
  };
});

const goAddPage = () => {
  navigateTo('/add');
};
</script>

<template>
  <BaseH1>やること管理</BaseH1>
  <BaseH2>一覧</BaseH2>
  <ul>
    <li v-for="link in links" :key="link.url">
      <NuxtLink :to="link.url">{{ link.text }}</NuxtLink>
    </li>
  </ul>

  <div class="text-right">
    <ButtonPrimary :onClick="goAddPage">タスクの追加</ButtonPrimary>
  </div>
</template>
