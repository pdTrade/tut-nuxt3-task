<script setup lang="ts">
type TaskModel = {
  name: string;
  items: {
    name: string;
    amount: "" | number;
    unit: string;
  }[];
};

const props = defineProps<{
  modelValue: TaskModel;
  id: string;
}>();
const emits = defineEmits<{
  (e: "update:modelValue", value: TaskModel): void;
}>();

const formData = computed({
  get: () => props.modelValue,
  set: (value) => emits("update:modelValue", value),
});

const addItem = () => {
  formData.value.items.push({
    name: "",
    amount: "",
    unit: "",
  });
};

const removeItem = (index: number) => {
  formData.value.items.splice(index, 1);
};

const dbName = "taskDB";
const storeName = "taskStore";

const submit = () => {
  const { name, items } = formData.value;

  if (
    !name ||
    items.some((v) => !v.name || !Number.isFinite(v.amount) || !v.unit)
  ) {
    alert("フォームを全て入力してください");
    return;
  }

  const openReq = indexedDB.open(dbName);

  openReq.onsuccess = (event) => {
    const db = event.target.result;
    const trans = db.transaction(storeName, "readwrite");
    const store = trans.objectStore(storeName);

    const id = props.id;

    const putReq = store.put({
      id,
      name,
      items: items.map((v) => ({
        name: v.name,
        amount: v.amount,
        unit: v.unit,
      })),
    });

    putReq.onsuccess = () => {
      alert("保存に成功しました。");
    };
  };

  navigateTo("/");
};

const goBack = () => {
  navigateTo("/");
};
</script>

<template>
  <div class="grid gap-4">
    <label>
      タスク名
      <InputText v-model="formData.name"></InputText>
    </label>
    <div
      v-for="(item, index) in formData.items"
      class="rounded-md bg-cyan-100 p-4 shadow-md"
    >
      <div class="text-right">
        <ButtonDanger :on-click="() => removeItem(index)"
          >手順{{ index + 1 }}を削除する</ButtonDanger
        >
      </div>
      <div class="grid gap-2 sm:grid-cols-3">
        <label>
          手順{{ index + 1 }}
          <InputText v-model="item.name" />
        </label>
        <label>
          想定時間{{ index + 1 }}
          <InputNum v-model="item.amount" />
        </label>
        <label>
          時間単位{{ index + 1 }}
          <InputText v-model="item.unit" />
        </label>
      </div>
    </div>
    <div>
      <ButtonPrimary :onClick="addItem">手順を追加</ButtonPrimary>
    </div>
    <label> 内容 </label>
    <div class="space-x-4 text-right">
      <ButtonSecondary :onClick="goBack">一覧に戻る</ButtonSecondary>
      <ButtonPrimary :onClick="submit">保存</ButtonPrimary>
    </div>
  </div>
</template>
