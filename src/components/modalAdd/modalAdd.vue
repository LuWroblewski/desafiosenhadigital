<template>
  <button class="btn btn-soft" onclick="addmodal.showModal()"><ListPlus /> Adicionar item</button>

  <dialog id="addmodal" class="modal modal-bottom sm:modal-middle">
    <div class="modal-box">
      <h3 class="text-lg font-bold">Adicionar item a lista</h3>
      <div class="w-full space-y-2.5 mt-4">
        <label class="floating-label">
          <span>Titulo</span>
          <input v-model="title" type="text" placeholder="Titulo" required class="input w-full" />
        </label>
        <label class="floating-label">
          <span>Descrição</span>
          <input v-model="description" type="text" placeholder="Descrição" required class="input w-full" />
        </label>
        <label class="input w-full">
          <span class="label">Data de Vencimento</span>
          <input v-model="data" type="date" required />
        </label>

        <div class="flex justify-end">
          <button class="btn btn-outline btn-primary" @click="addItem">Enviar <ArrowBigRight /></button>
        </div>
      </div>
    </div>

    <form method="dialog" class="modal-backdrop">
      <button>close</button>
    </form>
  </dialog>
</template>

<script setup>
import { ArrowBigRight, ListPlus } from 'lucide-vue-next';
import { ref } from 'vue';

const emit = defineEmits(['item-added']);

const title = ref('');
const description = ref('');
const data = ref('');
const modal = ref(null);

function addItem() {
  if (!title.value || !description.value || !data.value) {
    alert('Preencha todos os campos!');
    return;
  }

  const novoItem = {
    id: Date.now(),
    title: title.value,
    description: description.value,
    data: data.value,
    status: 'pendente',
  };

  emit('item-added', novoItem);

  title.value = '';
  description.value = '';
  data.value = '';

  addmodal.close();
}
</script>
