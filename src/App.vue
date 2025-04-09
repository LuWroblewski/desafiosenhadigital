<script setup>
import { ref, computed, onMounted } from 'vue';
import List from './components/list/list.vue';
import StatusFilter from './components/filter/filter.vue';
import TextFilter from './components/textFilter/textFilter.vue';
import ModalAdd from './components/modal_add/modal_add.vue';

const items = ref([]);

onMounted(() => {
  const saved = localStorage.getItem('list');
  if (saved) {
    items.value = JSON.parse(saved);
  }
});

const filtroTexto = ref('');
const filtroStatus = ref('todos');

function filterByTitle(texto) {
  filtroTexto.value = texto;
}

function filterList(status) {
  filtroStatus.value = status;
}

const filteredItems = computed(() => {
  return items.value.filter((item) => {
    const matchTexto =
      item.title.toLowerCase().includes(filtroTexto.value.toLowerCase()) ||
      item.description.toLowerCase().includes(filtroTexto.value.toLowerCase());

    const matchStatus = filtroStatus.value === 'todos' || item.status === filtroStatus.value;

    return matchTexto && matchStatus;
  });
});

function saveToStorage() {
  localStorage.setItem('list', JSON.stringify(items.value));
}

function handleUpdateItem(updatedItem) {
  const index = items.value.findIndex((i) => i.id === updatedItem.id);
  if (index !== -1) {
    items.value[index] = updatedItem;
    saveToStorage();
  }
}

function handleDeleteItem(item) {
  const index = items.value.findIndex((i) => i.id === item.id);
  if (index !== -1) {
    items.value.splice(index, 1);
    saveToStorage();
  }
}

function handleFinishItem(item) {
  const index = items.value.findIndex((i) => i.id === item.id);
  if (index !== -1) {
    items.value[index].status = 'concluido';
    saveToStorage();
  }
}

function addItem(novoItem) {
  items.value.push(novoItem);
  saveToStorage();
}
</script>

<template>
  <main class="mt-20 space-y-6">
    <p class="text-4xl text-center">Itens</p>
    <div class="flex justify-center items-center gap-4">
      <TextFilter @search="filterByTitle" />
      <StatusFilter @status-change="filterList" />
      <ModalAdd @item-added="addItem" />
    </div>

    <List
      :items="filteredItems"
      @update-item="handleUpdateItem"
      @delete-item="handleDeleteItem"
      @finish-item="handleFinishItem"
    />
  </main>
</template>
