<template>
  <tr>
    <td>{{ index + 1 }}</td>
    <td><input v-model="editedItem.title" class="input input-bordered w-full" /></td>
    <td><input v-model="editedItem.description" class="input input-bordered w-full" /></td>
    <td><input type="date" v-model="editedItem.data" class="input input-bordered w-full" /></td>
    <td class="font-semibold text-blue-600">pendente</td>
    <td class="flex gap-2">
      <button @click="onSave" class="btn btn-success btn-sm">Salvar</button>
      <button @click="$emit('cancelar')" class="btn btn-ghost btn-sm">Cancelar</button>
    </td>
  </tr>
</template>

<script setup>
import { reactive, toRefs, watch } from 'vue';

const props = defineProps({
  index: Number,
  item: Object,
});

const emits = defineEmits(['salvar', 'cancelar']);

const editedItem = reactive({
  title: props.item.title,
  description: props.item.description,
  data: props.item.data,
});

watch(
  () => props.item,
  (novo) => {
    editedItem.title = novo.title;
    editedItem.description = novo.description;
    editedItem.data = novo.data;
  }
);

function onSave() {
  emits('salvar', { ...editedItem });
}
</script>
