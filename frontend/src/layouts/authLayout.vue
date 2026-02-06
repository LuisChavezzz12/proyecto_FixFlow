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

                <div class="brand-header">
                    <h1>FixFlow</h1>
                    <p class="form-title">{{ formTitle }}</p>
                </div>

                <slot></slot>

            </div>
        </div>
    </div>
</template>

<script setup>
// Definimos qué textos puede recibir este layout
defineProps({
    title: { type: String, default: 'Mantén el Control' },
    subtitle: { type: String, default: 'Gestiona tu taller de principio a fin.' },
    formTitle: { type: String, default: 'Bienvenido' }
});
</script>

<style scoped>
/* PEGA AQUÍ TODO EL CSS QUE HICIMOS ANTES PARA EL LOGIN */
/* (Copiado de la respuesta anterior para que funcione directo) */

.split-screen {
    display: flex;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background: white;
}

/* IZQUIERDA */
.left-pane {
    width: 45%;
    background-color: #1e3a8a;
    background-image: url();
    background-size: cover;
    background-position: center;
    position: relative;
}

.overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    top: 0;
    background: linear-gradient(to top, rgba(0, 0, 0, 0.8), transparent);
    display: flex;
    align-items: flex-end;
    padding: 4rem;
}

.overlay-content h2 {
    color: white;
    font-size: 2.5rem;
    margin: 0 0 10px 0;
}

.overlay-content p {
    color: rgba(255, 255, 255, 0.8);
    font-size: 1.1rem;
}

/* CAMBIO AQUÍ: Fondo gris claro para que resalte la tarjeta */
.right-pane {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 2rem;
  background: #f8fafc; /* <--- ANTES ERA WHITE */
}

.auth-container {
    width: 100%;
    max-width: 400px;
}

/* HEADER MARCA */
.brand-header {
    margin-bottom: 2rem;
}

.logo {
    font-size: 2rem;
    margin-bottom: 0.5rem;
}

.brand-header h1 {
    font-size: 1.8rem;
    color: #111827;
    margin: 0;
    font-weight: 700;
}

.form-title {
    color: #64748b;
    margin-top: 5px;
}

@media (max-width: 900px) {
    .left-pane {
        display: none;
    }

    .right-pane {
        width: 100%;
    }
}
</style>