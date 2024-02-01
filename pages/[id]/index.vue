<script setup lang="ts">
import { onMounted, transformVNodeArgs } from 'vue';

type TaskModel = {
  name: string;
  items: {
    name: string;
    amount: '' | number;
    unit: string;
  }[];
};

const route = useRoute();
const id = route.params.id;

const dbName = 'taskDB';
const storeName = 'taskStore';

const record = ref<TaskModel | null>(null);

onMounted(() => {
  const openReq = indexedDB.open(dbName);
  openReq.onsuccess = (event) => {
    const db = event.target.result;

    const trans = db.transaction(storeName, 'readonly');

    const store = trans.objectStore(storeName);

    const getReq = store.get(id);

    getReq.onsuccess = (event) => {
      record.value = event.target.result;

      if (!record.value) {
        alert('タスクの情報が見つかりません');
        navigateTo('/');
      }
    };
  };
});

const goBack = () => {
  navigateTo('/');
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
    <ButtonSecondary :onClick="goBack">一覧に戻る</ButtonSecondary>
  </div>
  <BaseH2>詳細</BaseH2>

  <div>
    <div v-for="(item, index) in record?.items">
      {{ item.name }}
      {{ item.amount }}
      {{ item.unit }}
    </div>
    <div class="">
      <ButtonDanger :onClick="deleteItem">
        このタスクを削除する
      </ButtonDanger>
    </div>
  </div>


</template>