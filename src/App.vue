<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <header class="app-header">
    <span class="app-title">Mis Tareas</span>
  </header>
  <main class="app-main">
    <RouterView />
  </main>
</template>

<style scoped>
.app-header {
  padding: 18px 24px;
  background: var(--color-card);
  border-bottom: 1px solid var(--color-border);
}

.app-title {
  font-size: 18px;
  font-weight: 600;
  color: var(--color-primary);
  letter-spacing: -0.3px;
}

.app-main {
  flex: 1;
  padding: 36px 24px;
}
</style>
