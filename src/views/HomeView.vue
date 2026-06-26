<script setup>
import { ref, computed, watch } from "vue";

const STORAGE_KEY = "tareas";

const nuevaTarea = ref("");
const tareas = ref(JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"));
const filtro = ref("todas");

watch(tareas, (val) => localStorage.setItem(STORAGE_KEY, JSON.stringify(val)), { deep: true });

const tareasFiltradas = computed(() => {
  if (filtro.value === "pendientes") return tareas.value.filter((t) => !t.hecha);
  if (filtro.value === "hechas") return tareas.value.filter((t) => t.hecha);
  return tareas.value;
});

function agregar() {
  if (!nuevaTarea.value.trim()) return;
  tareas.value.push({ id: Date.now(), titulo: nuevaTarea.value, hecha: false });
  nuevaTarea.value = "";
}

function eliminar(id) {
  tareas.value = tareas.value.filter((t) => t.id !== id);
}
</script>

<template>
  <div class="card">
    <div class="input-row">
      <input
        v-model="nuevaTarea"
        @keyup.enter="agregar"
        placeholder="Nueva tarea..."
        class="task-input"
      />
      <button @click="agregar" class="btn-add">Añadir</button>
    </div>

    <div class="filters">
      <button
        v-for="f in ['todas', 'pendientes', 'hechas']"
        :key="f"
        @click="filtro = f"
        :class="['filter-btn', { active: filtro === f }]"
      >
        {{ f.charAt(0).toUpperCase() + f.slice(1) }}
      </button>
    </div>

    <ul class="task-list">
      <li v-if="tareasFiltradas.length === 0" class="empty-state">
        No hay tareas aquí.
      </li>
      <li v-for="t in tareasFiltradas" :key="t.id" :class="['task-item', { done: t.hecha }]">
        <input type="checkbox" v-model="t.hecha" class="task-checkbox" />
        <span class="task-title">{{ t.titulo }}</span>
        <button @click="eliminar(t.id)" class="btn-delete" aria-label="Eliminar">
          <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14H6L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M9 6V4h6v2"/>
          </svg>
        </button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.card {
  max-width: 560px;
  margin: 0 auto;
  background: var(--color-card);
  border-radius: 16px;
  padding: 28px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06), 0 4px 16px rgba(0, 0, 0, 0.04);
}

.input-row {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.task-input {
  flex: 1;
  padding: 10px 14px;
  border: 1.5px solid var(--color-border);
  border-radius: 8px;
  font-size: 15px;
  outline: none;
  background: var(--color-card);
  color: var(--color-text);
  transition: border-color 0.15s;
}

.task-input:focus {
  border-color: var(--color-primary);
}

.task-input::placeholder {
  color: var(--color-text-muted);
}

.btn-add {
  padding: 10px 20px;
  background: var(--color-primary);
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 15px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.15s;
  white-space: nowrap;
}

.btn-add:hover {
  background: var(--color-primary-hover);
}

.filters {
  display: flex;
  gap: 6px;
  margin-bottom: 20px;
}

.filter-btn {
  padding: 5px 14px;
  border-radius: 20px;
  border: 1.5px solid var(--color-border);
  background: transparent;
  color: var(--color-text-muted);
  font-size: 13px;
  cursor: pointer;
  transition: all 0.15s;
}

.filter-btn:hover:not(.active) {
  border-color: var(--color-primary);
  color: var(--color-primary);
}

.filter-btn.active {
  background: var(--color-primary);
  border-color: var(--color-primary);
  color: #fff;
}

.task-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 14px;
  border: 1px solid var(--color-border);
  border-radius: 10px;
  transition: box-shadow 0.15s;
}

.task-item:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.07);
}

.task-checkbox {
  width: 17px;
  height: 17px;
  accent-color: var(--color-primary);
  cursor: pointer;
  flex-shrink: 0;
}

.task-title {
  flex: 1;
  color: var(--color-text);
  font-size: 15px;
  transition: color 0.15s;
}

.task-item.done .task-title {
  text-decoration: line-through;
  color: var(--color-text-muted);
}

.btn-delete {
  background: none;
  border: none;
  color: var(--color-text-muted);
  cursor: pointer;
  padding: 5px;
  border-radius: 6px;
  transition: color 0.15s, background 0.15s;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.btn-delete:hover {
  color: var(--color-danger);
  background: var(--color-danger-hover);
}

.empty-state {
  text-align: center;
  color: var(--color-text-muted);
  font-size: 14px;
  padding: 28px 0;
}
</style>
