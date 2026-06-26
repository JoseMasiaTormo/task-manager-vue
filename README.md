# Task Manager

Gestor de tareas minimalista construido con Vue 3. Sin backends, sin complicaciones — todo vive en el navegador.

## Características

- **Añadir tareas** escribiendo y pulsando Enter o el botón
- **Marcar como hechas** con un checkbox; el texto se tacha automáticamente
- **Filtrar** entre todas, pendientes y hechas
- **Eliminar** tareas individualmente
- **Persistencia con localStorage** — las tareas se guardan en el navegador y sobreviven al F5 y al cierre de la pestaña

## Stack

- [Vue 3](https://vuejs.org/) con `<script setup>` y Composition API
- [Vue Router](https://router.vuejs.org/)
- [Pinia](https://pinia.vuejs.org/)
- [Vite](https://vite.dev/)

## Instalación

```bash
npm install
npm run dev
```

## Build para producción

```bash
npm run build
```
