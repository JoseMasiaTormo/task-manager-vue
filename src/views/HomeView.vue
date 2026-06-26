<script setup>
import { ref, computed } from "vue";

const nuevaTarea = ref("");
const tareas = ref([]);
const filtro = ref("todas");

const tareasFiltradas = computed(() => {
  if (filtro.value === "pendientes") return tareas.value.filter((t) => !t.hecha);
  if (filtro.value === "hechas") return tareas.value.filter((t) => t.hecha);
  return tareas.value;
});

function agregar() {
  if (!nuevaTarea.value.trim()) return;
  tareas.value.push({
    id: Date.now(),
    titulo: nuevaTarea.value,
    hecha: false,
  });
  nuevaTarea.value = "";
}

function eliminar(id) {
  tareas.value = tareas.value.filter((t) => t.id !== id);
}
</script>

<template>
  <div>
    <input v-model="nuevaTarea" @keyup.enter="agregar" placeholder="Nueva tarea..." />
    <button @click="agregar">Añadir</button>

    <select v-model="filtro">
      <option value="todas">Todas</option>
      <option value="pendientes">Pendientes</option>
      <option value="hechas">Hechas</option>
    </select>

    <ul>
      <li v-for="t in tareasFiltradas" :key="t.id">
        <input type="checkbox" v-model="t.hecha" />
        <span :style="{ textDecoration: t.hecha ? 'line-through' : 'none' }">
          {{ t.titulo }}
        </span>
        <button @click="eliminar(t.id)">✕</button>
      </li>
    </ul>
  </div>
</template>
