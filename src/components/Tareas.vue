<template>
  <div class="tareas-container">
    <h2>Contador de Tareas</h2>

    <!-- Mostramos el total de tareas con clase dinámica -->
    <p :class="tareasClass">Tareas totales: {{ tareas.length }}</p>

    <!-- Editor para nuevas tareas -->
    <form class="nuevo-form" @submit.prevent="agregarTarea">
      <input
        v-model="nueva"
        type="text"
        placeholder="Escribe una nueva tarea y presiona Enter o click ➕"
        class="input-nueva"
        @keyup.enter="agregarTarea"
        ref="inputNueva"
        aria-label="Nueva tarea"
      />
      <button type="submit" :disabled="!nuevaTrim" class="btn-agregar">➕ Agregar tarea</button>
    </form>
    <p v-if="error" class="error-msg">{{ error }}</p>

    <!-- Insertamos el componente nieto y le pasamos la lista como prop -->
    <ListaTareas :tareas="tareas" />
  </div>
</template>

<script setup>
// Importamos las herramientas de Vue
import { ref, computed } from 'vue'
import ListaTareas from './ListaTareas.vue'

// Creamos un array reactivo de tareas
const tareas = ref(['Estudiar Vue 3', 'Preparar el parcial', 'Repasar Composition API'])

// Campo de edición para la nueva tarea
const nueva = ref('')
const error = ref('')

// Computed para saber si el campo no está vacío después de trim
const nuevaTrim = computed(() => nueva.value.trim().length > 0)

// Referencia al input para enfocarlo después de agregar
const inputNueva = ref(null)

// Función que agrega una nueva tarea con validación
const agregarTarea = () => {
  // Limpiamos mensaje de error previo
  error.value = ''

  if (!nuevaTrim.value) {
    error.value = 'La tarea no puede estar vacía.'
    return
  }

  // Añadimos la tarea (usar el texto tal cual escrito)
  tareas.value.push(nueva.value.trim())

  // Limpiamos el campo y devolvemos el foco
  nueva.value = ''
  if (inputNueva.value && inputNueva.value.focus) inputNueva.value.focus()
}

// Clase dinámica: cambia según la cantidad de tareas
const tareasClass = computed(() => {
  return tareas.value.length >= 5 ? 'muchas-tareas' : 'pocas-tareas'
})
</script>

<style scoped>
.tareas-container {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 12px;
  width: 350px;
  margin: 0 auto;
}

button {
  margin-top: 10px;
  padding: 8px 12px;
  border: none;
  border-radius: 6px;
  background-color: #42b983;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #2e9b72;
}

.pocas-tareas {
  color: blue;
}

.muchas-tareas {
  color: red;
  font-weight: bold;
}

.nuevo-form {
  display: flex;
  gap: 8px;
  margin-top: 12px;
}

.input-nueva {
  flex: 1 1 auto;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ddd;
}

.btn-agregar[disabled] {
  opacity: 0.6;
  cursor: not-allowed;
}

.error-msg {
  color: #c81414;
  margin-top: 8px;
  font-size: 0.9rem;
}
</style>
