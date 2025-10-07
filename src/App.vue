<template>
  <div class="app">
    <h1>Aplicación de Tareas</h1>
    <!-- Insertamos el componente hijo -->
    <Tareas />
  </div>
</template>

<script setup>
import Tareas from './components/Tareas.vue'
</script>

<style>
.app {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 40px;
}
</style>
