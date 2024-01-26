<script setup lang="ts">
type TaskModel = {
  name: string;
  items: {
    name: string;
    amount: '' | number;
    unit: string;
  }[];
};

const props = defineProps<{
  modelValue: TaskModel;
  id: string;
}>();
const emits = defineEmits<{
  (e: "update:modelValue", value: TaskModel): void
}>();

const formData = computed({
  get: () => props.modelValue,
  set: (value) => emits("update:modelValue", value)
});

const addItem = () => {
  formData.value.items.push({
    name: '',
    amount: '',
    unit: '',
  });
};


const submit = () => { };

</script>

<template>
  <div class="">
    <label>
      タスク名
      <InputText v-model="formData.name"></InputText>
    </label>
    <div
      v-for="(item, index) in formData.items"
    >
      <label>
        手順{{ index + 1 }}
        <InputText v-model="item.name"/>
      </label>
      <label>
        想定時間{{ index + 1 }}
        <InputNum v-model="item.amount"/>
      </label>
      <label>
        時間単位{{ index + 1 }}
        <InputText v-model="item.unit"/>
      </label>
    </div>
    <div>
      <ButtonPrimary :onClick="addItem">追加</ButtonPrimary>
    </div>
    <div class="text-right">
      <ButtonPrimary :onClick="submit">保存</ButtonPrimary>
    </div>
  </div>
</template>
