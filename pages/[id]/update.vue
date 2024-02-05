<script setup lang="ts">
import { onMounted } from 'vue';

const route = useRoute();
const id = route.params.id;

const dbName = 'taskDB';
const storeName = 'taskStore';

const formData = reactive({
  name: '',
  items: [],
});

onMounted(() => {
  const openReq = indexedDB.open(dbName);
  openReq.onsuccess = (event) => {
    const db = event.target.result;

    const trans = db.transaction(storeName, 'readonly');

    const store = trans.objectStore(storeName);

    const getReq = store.get(id);

    getReq.onsuccess = (event) => {
      const res = event.target.result;

      if (!res) {
        alert('タスクの情報が見つかりません');
        navigateTo('/');
      }

      formData.name = res.name;
      formData.items = res.items;
    };
  };
});

const goBack = () => {
  navigateTo('/${id}');
};

const deleteItem = () => {
  if (!confirm('本当に削除しますか')) {
    return;
  }

  const openReq = indexedDB.open(dbName);
  openReq.onsuccess = (event) => {
    const db = event.target.result;

    const trans = db.transaction(storeName, 'readwrite');

    const store = trans.objectStore(storeName);

    const deleteReq = store.delete(id);

    deleteReq.onsuccess = () => {
        navigateTo('/');
    };
  };
}
</script>

<template>
  <BaseH1>{{ record?.name }}</BaseH1>
  <div class="text-right">
    <ButtonSecondary :onClick="goBack">詳細ページに戻る</ButtonSecondary>
  </div>
  <BaseH2>詳細</BaseH2>

  <div>
    <TaskForm v-model="formData" :id="id" />
  </div>


</template>