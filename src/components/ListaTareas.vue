<template>
  <div class="lista-tareas">
    <h3>🧾 Lista de Tareas</h3>

    <!-- Si hay tareas, las mostramos -->
    <ul v-if="tareas.length > 0">
      <li v-for="(tarea, index) in tareas" :key="index" class="tarea-item">
        <div class="tarea-text">
          <strong>{{ index + 1 }}.</strong>
          <template v-if="editingIndex === index">
            <input v-model="editText" class="input-edit" />
            <button @click="confirmEdit(index)" class="btn-small">Guardar</button>
            <button @click="cancelEdit" class="btn-small">Cancelar</button>
          </template>
          <template v-else>
            <span class="tarea-desc">{{ tarea }}</span>
            <button @click="startEdit(index, tarea)" class="btn-small">✏️ Editar</button>
            <button @click="$emit('delete', index)" class="btn-small danger">🗑️ Eliminar</button>
          </template>
        </div>
      </li>
    </ul>

    <!-- Si no hay tareas -->
    <p v-else>No hay tareas pendientes 😎</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Props
defineProps({
  tareas: {
    type: Array,
    required: true,
  },
})

// Emits
const emit = defineEmits(['delete', 'edit'])

// Edición inline
const editingIndex = ref(-1)
const editText = ref('')

const startEdit = (index, texto) => {
  editingIndex.value = index
  editText.value = texto
}

const confirmEdit = (index) => {
  if (editText.value.trim().length === 0) return
  emit('edit', { index, text: editText.value.trim() })
  editingIndex.value = -1
  editText.value = ''
}

const cancelEdit = () => {
  editingIndex.value = -1
  editText.value = ''
}
</script>

<style scoped>
.lista-tareas {
  margin-top: 20px;
  text-align: left;
  background-color: #fff;
  padding: 15px;
  border-radius: 8px;
}

ul {
  list-style-type: disc;
  padding-left: 20px;
}

li {
  margin: 6px 0;
}

.tarea-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.tarea-text {
  display: flex;
  gap: 8px;
  align-items: center;
}

.btn-small {
  margin-left: 6px;
  padding: 4px 8px;
  border-radius: 6px;
  border: none;
  background: #42b983;
  color: white;
  cursor: pointer;
}

.btn-small.danger {
  background: #ef4444;
}

.input-edit {
  padding: 4px 8px;
  border-radius: 6px;
  border: 1px solid #ddd;
}
</style>
