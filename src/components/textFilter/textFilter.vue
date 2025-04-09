<template>
  <input
    v-model="searchText"
    @input="emitSearch"
    type="text"
    placeholder="Buscar por título..."
    class="input input-bordered w-full max-w-xs"
  />
</template>

<script setup>
import { ref } from 'vue';

const searchText = ref('');
const emit = defineEmits(['search']);

function emitSearch() {
  emit('search', searchText.value);
}
</script>
