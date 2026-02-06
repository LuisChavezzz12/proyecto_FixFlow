<template>
    <div class="auth-wrapper">
        <div class="login-card">
            <header>
                <h1>FixFlow</h1>
                <p>Acceso al Sistema</p>
            </header>

            <form @submit.prevent="handleLogin">
                <div class="form-group">
                    <label for="email">Correo Electrónico</label>
                    <input id="email" v-model="email" type="email" placeholder="correo@ejemplo.com"
                        :class="{ 'border-error': emailError }">
                    <span class="error-text">{{ emailError }}</span>
                </div>

                <div class="form-group">
                    <label for="password">Contraseña</label>
                    <input id="password" v-model="password" type="password" placeholder="••••••••"
                        :class="{ 'border-error': passError }">
                    <span class="error-text">{{ passError }}</span>
                </div>

                <button :disabled="hasErrors || isLoading" class="btn-primary">
                    {{ isLoading ? 'Entrando...' : 'Ingresar' }}
                </button>
            </form>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const email = ref('');
const password = ref('');
const isLoading = ref(false);

// Validaciones en tiempo real usando Computed Properties
const emailError = computed(() => {
    if (!email.value) return '';
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email.value) ? '' : 'Formato de correo inválido';
});

const passError = computed(() => {
    if (!password.value) return '';
    return password.value.length < 8 ? 'Mínimo 8 caracteres' : '';
});

const hasErrors = computed(() => {
    return !!(emailError.value || passError.value || !email.value || !password.value);
});

const handleLogin = async () => {
    if (hasErrors.value) return;
    isLoading.value = true;
    // Simulación de acceso seguro 
    setTimeout(() => {
        isLoading.value = false;
        router.push('/inventory');
    }, 1000);
};
</script>

<style scoped>
.auth-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: #fafafa;
    font-family: sans-serif;
}

.login-card {
    background: white;
    padding: 2rem;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
    width: 100%;
    max-width: 360px;
}

header {
    text-align: center;
    margin-bottom: 1.5rem;
}

h1 {
    color: #2563eb;
    margin: 0;
    font-size: 1.8rem;
}

p {
    color: #666;
    font-size: 0.9rem;
    margin: 5px 0 0;
}

.form-group {
    margin-bottom: 1rem;
    height: 80px;
}

/* Altura fija para evitar saltos de UI */
label {
    color: black;
    display: block;
    font-size: 0.85rem;
    font-weight: 600;
    margin-bottom: 4px;
}

input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    box-sizing: border-box;
    transition: border-color 0.2s;
}

input:focus {
    border-color: #2563eb;
    outline: none;
}

.border-error {
    border-color: #ef4444;
}

.error-text {
    color: #ef4444;
    font-size: 0.75rem;
    display: block;
    margin-top: 4px;
    min-height: 1em;
}

.btn-primary {
    width: 100%;
    padding: 12px;
    background: #2563eb;
    color: white;
    border: none;
    border-radius: 4px;
    font-weight: 600;
    cursor: pointer;
}

.btn-primary:disabled {
    background: #94a3b8;
    cursor: not-allowed;
}
</style>