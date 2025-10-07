<template>
  <div class="lista-tareas">
    <h3>Lista de Tareas</h3>

    <!-- Si hay tareas, las mostramos -->
    <ul v-if="tareas.length > 0">
      <li v-for="(tarea, index) in tareas" :key="index">
        {{ tarea }}
      </li>
    </ul>

    <!-- Si no hay tareas -->
    <p v-else>No hay tareas pendientes 😎</p>
  </div>
</template>

<script setup>
// Importamos defineProps para recibir la prop desde el componente padre
defineProps({
  tareas: {
    type: Array,
    required: true,
  },
})
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
</style>
