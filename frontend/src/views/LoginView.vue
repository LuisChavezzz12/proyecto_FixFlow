<template>
    <AuthLayout title="Bienvenido de nuevo" subtitle="Tu taller te espera. Inicia sesión para continuar." formTitle=" ">
        <div class="login-card">
            <div class="card-header">

                <p>Acceso al Sistema</p>
            </div>

            <form @submit.prevent="handleLogin" autocomplete="off">

                <div class="form-group">
                    <label for="email">Correo</label>
                    <div class="input-wrapper">
                        <input id="email" v-model="email" type="email" placeholder="admin@fixflow.com"
                            :class="getInputClass('email')" @blur="touched.email = true" @input="touched.email = true">
                        <span v-if="touched.email" class="status-icon">
                            <i v-if="isEmailValid" class="valid">✔</i>
                            <i v-else class="invalid">✖</i>
                        </span>
                    </div>
                    <span v-if="touched.email && !isEmailValid" class="error-msg">Formato de correo inválido.</span>
                </div>

                <div class="form-group">
                    <label for="password">Contraseña</label>
                    <div class="input-wrapper">
                        <input id="password" v-model="password" :type="showPassword ? 'text' : 'password'"
                            placeholder="Ingresa tu contraseña" :class="getInputClass('password')"
                            @blur="touched.password = true" @input="touched.password = true">
                        <button type="button" class="toggle-btn" @click="showPassword = !showPassword">
                            {{ showPassword ? 'Ocultar' : 'Ver' }}
                        </button>
                    </div>

                    <div class="security-pills">
                        <span :class="{ 'pill-ok': passwordRules.length }">8+ Caracteres</span>
                        <span :class="{ 'pill-ok': passwordRules.uppercase }">Mayúscula</span>
                        <span :class="{ 'pill-ok': passwordRules.number }">Número</span>
                        <span :class="{ 'pill-ok': passwordRules.special }">Símbolo</span>
                    </div>
                </div>

                <button type="submit" class="btn-submit" :disabled="!isFormValid || isLoading">
                    <span v-if="isLoading" class="loader"></span>
                    <span v-else>Ingresar</span>
                </button>

                <div class="form-footer">
                    <p>¿No tienes cuenta?</p>
                    <router-link to="/registro" class="link">Regístrate aquí</router-link>
                </div>

            </form>
        </div>
    </AuthLayout>
</template>

<script setup>
import { ref, computed, reactive } from 'vue';
import { useRouter } from 'vue-router';
import AuthLayout from '../layouts/authLayout.vue'; // Asegúrate que la A sea mayúscula si el archivo lo es

const router = useRouter();

// Estados
const email = ref('');
const password = ref('');
const showPassword = ref(false);
const isLoading = ref(false);

const touched = reactive({
    email: false,
    password: false
});

// --- Validaciones ---
const isEmailValid = computed(() => {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email.value);
});

// Reglas de complejidad
const passwordRules = computed(() => {
    const pwd = password.value;
    return {
        length: pwd.length >= 8,
        uppercase: /[A-Z]/.test(pwd),
        number: /[0-9]/.test(pwd),
        special: /[!@#$%^&*(),.?":{}|<>]/.test(pwd)
    };
});

const isPasswordSecure = computed(() => {
    const r = passwordRules.value;
    return r.length && r.uppercase && r.number && r.special;
});

const isFormValid = computed(() => isEmailValid.value && isPasswordSecure.value);

const getInputClass = (field) => {
    if (field === 'email') {
        return touched.email ? (isEmailValid.value ? 'input-valid' : 'input-invalid') : '';
    }
    if (field === 'password') {
        return touched.password ? (isPasswordSecure.value ? 'input-valid' : 'input-invalid') : '';
    }
    return '';
};

const handleLogin = async () => {
    if (!isFormValid.value) return;
    isLoading.value = true;

    setTimeout(() => {
        isLoading.value = false;
        router.push('/home');
    }, 1000);
};
</script>

<style scoped>
/* TARJETA FLOTANTE (Restaurada) */
.login-card {
    background: white;
    width: 100%;
    max-width: 400px;
    padding: 2.5rem;
    border-radius: 16px;
    /* Sombra profunda para destacar sobre el fondo gris del layout */
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
    border-top: 5px solid #2563eb;
}

.card-header {
    text-align: center;
    margin-bottom: 2rem;
}

.card-header h1 {
    margin: 0;
    font-size: 1.8rem;
    color: #111827;
    font-weight: 700;
}

.card-header p {
    margin-top: 6px;
    color: #6b7280;
    font-size: 0.95rem;
}

/* Inputs y Grupos */
.form-group {
    margin-bottom: 1.25rem;
}

label {
    display: block;
    font-size: 0.85rem;
    font-weight: 600;
    color: #374151;
    margin-bottom: 0.5rem;
}

.input-wrapper {
    position: relative;
}

input {
    width: 100%;
    padding: 0.75rem 1rem;
    padding-right: 2.5rem;
    border: 1px solid #e5e7eb;
    border-radius: 8px;
    font-size: 0.95rem;
    transition: all 0.2s ease;
    box-sizing: border-box;
    background-color: #f9fafb;
}

input:focus {
    outline: none;
    background-color: white;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

/* Estados Validación */
input.input-valid {
    border-color: #10b981;
    background-color: #f0fdf4;
}

input.input-invalid {
    border-color: #ef4444;
    background-color: #fef2f2;
}

.status-icon {
    position: absolute;
    right: 12px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 0.9rem;
}

.valid {
    color: #10b981;
}

.invalid {
    color: #ef4444;
}

.error-msg {
    font-size: 0.75rem;
    color: #ef4444;
    margin-top: 4px;
    display: block;
    font-weight: 500;
}

.toggle-btn {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    font-size: 0.75rem;
    color: #6b7280;
    cursor: pointer;
    font-weight: 600;
}

/* Pills */
.security-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 10px;
}

.security-pills span {
    font-size: 0.7rem;
    background-color: #f3f4f6;
    color: #9ca3af;
    padding: 4px 10px;
    border-radius: 999px;
    font-weight: 600;
    border: 1px solid #e5e7eb;
    transition: all 0.3s;
}

.security-pills span.pill-ok {
    background-color: #dcfce7;
    color: #15803d;
    border-color: #bbf7d0;
}

/* Botón */
.btn-submit {
    width: 100%;
    margin-top: 1rem;
    padding: 0.875rem;
    background: linear-gradient(to bottom, #2563eb, #1d4ed8);
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
    box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.2);
    transition: all 0.2s;
    display: flex;
    justify-content: center;
    align-items: center;
}

.btn-submit:hover:not(:disabled) {
    transform: translateY(-1px);
    box-shadow: 0 6px 8px -1px rgba(37, 99, 235, 0.3);
}

.btn-submit:disabled {
    background: #d1d5db;
    color: #9ca3af;
    cursor: not-allowed;
    box-shadow: none;
}

.form-footer {
    margin-top: 1.5rem;
    text-align: center;
    font-size: 0.9rem;
    color: #6b7280;
}

.link {
    color: #2563eb;
    text-decoration: none;
    font-weight: 600;
    margin-left: 5px;
}

.link:hover {
    text-decoration: underline;
}

.loader {
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 50%;
    border-top: 2px solid white;
    width: 16px;
    height: 16px;
    animation: spin 0.8s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }
}
</style>