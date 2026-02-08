<template>
  <div class="split-screen">
    
    <div class="left-pane">
      <div class="overlay">
        <div class="overlay-content">
          <h2>{{ title }}</h2>
          <p>{{ subtitle }}</p>
        </div>
      </div>
    </div>

    <div class="right-pane">
      <div class="auth-container">
        <slot></slot>
      </div>
    </div>

  </div>
</template>

<script setup>
defineProps({
  title: { type: String, default: 'Mantén el Control' },
  subtitle: { type: String, default: 'Gestiona tu taller de principio a fin.' }
  // Eliminamos formTitle porque el título lo maneja la tarjeta interna
});
</script>

<style scoped>
/* ESTRUCTURA PRINCIPAL */
.split-screen {
  display: flex;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: white;
}

/* IZQUIERDA (Imagen) */
.left-pane {
  display: none; /* Oculto en móvil */
  width: 50%;    /* 50% exacto */
  background-color: #1e3a8a; /* Color de fondo por si falla la imagen */
  /* Agregamos una imagen real de un taller/tech */
  background-image: url('https://images.unsplash.com/photo-1581092921461-eab62e97a782?q=80&w=2070&auto=format&fit=crop');
  background-size: cover;
  background-position: center;
  position: relative;
}

/* Mostrar solo en pantallas grandes */
@media (min-width: 900px) {
  .left-pane { display: block; }
}

.overlay {
  position: absolute;
  inset: 0; /* Shortcut para top, right, bottom, left: 0 */
  background: linear-gradient(to top, rgba(0, 0, 0, 0.85), transparent);
  display: flex;
  align-items: flex-end;
  padding: 4rem;
}

.overlay-content h2 {
  color: white;
  font-size: 2.5rem;
  font-weight: 700;
  margin: 0 0 10px 0;
  line-height: 1.1;
}

.overlay-content p {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1.1rem;
  max-width: 80%;
}

/* DERECHA (Fondo Gris) */
.right-pane {
  flex: 1; /* Toma el espacio restante (100% en móvil, 50% en escritorio) */
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  background: #f8fafc; /* Gris suave para resaltar la tarjeta blanca */
}

.auth-container {
  width: 100%;
  max-width: 440px; /* Un poco más ancho para que la tarjeta respire */
}
</style>