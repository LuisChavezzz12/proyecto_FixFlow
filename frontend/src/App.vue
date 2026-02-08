<script setup>
import { RouterView } from 'vue-router'
</script>

<template>
  <RouterView />
</template>

<style>

:root {
  /* Paleta de Colores FixFlow */
  --primary: #2563eb;        /* Azul Vibrante (Botones, Links) */
  --primary-hover: #1d4ed8;  /* Azul Oscuro (Hover) */
  --bg-app: #f3f4f6;         /* Fondo Gris Suave (Pantallas) */
  --text-main: #111827;      /* Texto Principal (Casi negro) */
  --text-muted: #6b7280;     /* Texto Secundario (Gris) */
  
  /* Estados */
  --success: #10b981;        /* Verde (Éxito) */
  --error: #ef4444;          /* Rojo (Error) */
  --warning: #f59e0b;        /* Naranja (Alerta) */
}

/* Reset Básico */
body {
  margin: 0;
  padding: 0;
  font-family: 'Inter', 'Segoe UI', system-ui, -apple-system, sans-serif;
  background-color: var(--bg-app);
  color: var(--text-main);
  -webkit-font-smoothing: antialiased; /* Suavizado de fuentes */
  -moz-osx-font-smoothing: grayscale;
}

/* Para que todos los elementos calculen bien su tamaño */
*, *::before, *::after {
  box-sizing: border-box;
}

/* Estilos base para inputs y botones */
input, button, select, textarea {
  font-family: inherit;
}

/* Scrollbar moderno (Opcional) */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}
::-webkit-scrollbar-track {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>