<template>
  <div class="flex justify-center w-full mt-6">
    <div class="w-full max-w-4xl overflow-x-auto">
      <table class="table w-full">
        <thead>
          <tr>
            <th>#</th>
            <th>Título</th>
            <th>Descrição</th>
            <th>Vencimento</th>
            <th>Status</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in props.items" :key="index">
            <th>{{ index + 1 }}</th>

            <td v-if="editingIndex === index">
              <input v-model="editedItem.title" class="input input-bordered input-sm w-full" />
            </td>
            <td v-else>{{ item.title }}</td>

            <td v-if="editingIndex === index">
              <input v-model="editedItem.description" class="input input-bordered input-sm w-full" />
            </td>
            <td v-else>{{ item.description }}</td>

            <td v-if="editingIndex === index">
              <input v-model="editedItem.data" class="input input-bordered input-sm w-full" type="date" />
            </td>
            <td v-else>{{ item.data }}</td>

            <td :class="item.status === 'pendente' ? 'text-warning font-semibold' : 'text-success font-semibold'">
              {{ item.status }}
            </td>

            <td class="space-x-2">
              <template v-if="editingIndex === index">
                <button class="btn btn-success" @click="saveEdit(index)">Salvar</button>
                <button class="btn btn-error btn-soft" @click="cancelEdit"><X /></button>
              </template>
              <template v-else>
                <div class="flex items-center gap-2">
                  <button
                    v-if="item.status === 'pendente'"
                    @click="$emit('finish-item', item)"
                    class="btn btn-success btn-soft flex items-center gap-2"
                  >
                    <Check class="w-4 h-4 md:hidden" />
                    <span class="hidden md:inline">Concluir</span>
                  </button>

                  <button @click="editItem(index)" class="btn btn-info btn-soft flex items-center gap-2">
                    <Pencil class="w-4 h-4 md:hidden" />
                    <span class="hidden md:inline">Editar</span>
                  </button>

                  <button @click="$emit('delete-item', item)" class="btn btn-error btn-soft flex items-center gap-2">
                    <X class="w-4 h-4 md:hidden" />
                    <span class="hidden md:inline">Excluir</span>
                  </button>
                </div>
              </template>
            </td>
          </tr>

          <tr v-if="props.items.length === 0">
            <td colspan="6" class="text-center text-gray-500">Nenhum item cadastrado</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { X, Check, Pencil } from 'lucide-vue-next';

const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['update-item', 'delete-item', 'finish-item']);

const editingIndex = ref(null);
const editedItem = ref({ title: '', description: '', data: '' });

function editItem(index) {
  editedItem.value = { ...props.items[index] };
  editingIndex.value = index;
}

function saveEdit(index) {
  const item = props.items[index];
  const updatedItem = {
    ...item,
    ...editedItem.value,
  };
  emit('update-item', updatedItem);
  editingIndex.value = null;
}

function cancelEdit() {
  editingIndex.value = null;
  editedItem.value = { title: '', description: '', data: '' };
}
</script>
