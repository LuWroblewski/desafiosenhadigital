<template>
  <div>
    <select v-model="selectedStatus" @change="emitFilter" class="select select-bordered w-full max-w-xs">
      <option value="" disabled selected hidden>Filtrar por status</option>
      <option value="todos">Todos</option>
      <option value="pendente">Pendente</option>
      <option value="concluido">Concluído</option>
    </select>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const selectedStatus = ref('');
const emit = defineEmits(['status-change']);

function emitFilter() {
  emit('status-change', selectedStatus.value);
}
</script>
