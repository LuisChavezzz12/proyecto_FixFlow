<template>
  <div class="dashboard-container">

    <header class="dashboard-header">
      <div class="welcome-text">
        <h1>Hola, Administrador 👋</h1>
        <p>Resumen de operaciones: {{ currentDate }}</p>
      </div>
      <button class="btn-primary" @click="goToNewOrder">
        + Nueva Recepción
      </button>
    </header>

    <div class="stats-grid">

      <div class="stat-card blue">
        <div class="icon-box">🔧</div>
        <div class="info">
          <h3>En Reparación</h3>
          <p class="number">12</p>
          <span class="detail">3 con retraso</span>
        </div>
      </div>

      <div class="stat-card green">
        <div class="icon-box">✅</div>
        <div class="info">
          <h3>Para Entrega</h3>
          <p class="number">5</p>
          <span class="detail">Esperando cliente</span>
        </div>
      </div>

      <div class="stat-card purple">
        <div class="icon-box">💰</div>
        <div class="info">
          <h3>Ventas del Día</h3>
          <p class="number">$4,250</p>
          <span class="detail">8 transacciones</span>
        </div>
      </div>

      <div class="stat-card orange">
        <div class="icon-box">⚠️</div>
        <div class="info">
          <h3>Stock Bajo</h3>
          <p class="number">4</p>
          <span class="detail">Refacciones críticas</span>
        </div>
      </div>
    </div>

    <div class="content-grid">

      <div class="panel">
        <div class="panel-header">
          <h2>Órdenes Urgentes</h2>
          <button class="link-btn">Ver Todo →</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr>
                <th>Folio</th>
                <th>Cliente</th>
                <th>Equipo</th>
                <th>Estado</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in urgentOrders" :key="order.id">
                <td class="folio">#{{ order.folio }}</td>
                <td>{{ order.cliente }}</td>
                <td>{{ order.equipo }}</td>
                <td>
                  <span :class="['badge', order.statusClass]">{{ order.status }}</span>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="panel actions-panel">
        <h2>Accesos Rápidos</h2>
        <div class="quick-actions">
          <button class="action-btn" @click="$router.push('/home/registro')">
            <span class="emoji">👤</span>
            <span>Registrar Usuario</span>
          </button>
          <button class="action-btn">
            <span class="emoji">📦</span>
            <span>Consultar Stock</span>
          </button>
          <button class="action-btn">
            <span class="emoji">🔍</span>
            <span>Buscar Orden</span>
          </button>
          <button class="action-btn">
            <span class="emoji">💵</span>
            <span>Corte de Caja</span>
          </button>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// Fecha actual formateada
const currentDate = new Date().toLocaleDateString('es-MX', {
  weekday: 'long', day: 'numeric', month: 'long'
});

// Datos simulados (Mock Data basados en tu esquema DB)
const urgentOrders = ref([
  { id: 1, folio: '00124', cliente: 'María López', equipo: 'iPhone 11', status: 'Diagnóstico', statusClass: 'warning' },
  { id: 2, folio: '00123', cliente: 'Carlos Ruiz', equipo: 'Samsung A52', status: 'En Espera', statusClass: 'danger' },
  { id: 3, folio: '00120', cliente: 'Ana Sofía', equipo: 'Laptop HP', status: 'Reparación', statusClass: 'info' },
]);

const goToNewOrder = () => {
  // Aquí redirigiremos al módulo de recepción cuando lo creemos
  alert("Ir a Módulo de Recepción");
};
</script>

<style scoped>
.dashboard-container {
  padding: 10px;
  font-family: 'Segoe UI', sans-serif;
}

/* Header */
.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.dashboard-header h1 {
  margin: 0;
  font-size: 1.8rem;
  color: #1e293b;
}

.dashboard-header p {
  margin: 5px 0 0;
  color: #64748b;
  text-transform: capitalize;
}

.btn-primary {
  background-color: #0f172a;
  color: white;
  padding: 10px 20px;
  border-radius: 8px;
  border: none;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.btn-primary:hover {
  background-color: #1e293b;
}

/* Tarjetas KPI */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.stat-card {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
  display: flex;
  align-items: center;
  gap: 1rem;
  border-left: 5px solid transparent;
}

.stat-card.blue {
  border-color: #3b82f6;
}

.stat-card.green {
  border-color: #22c55e;
}

.stat-card.purple {
  border-color: #a855f7;
}

.stat-card.orange {
  border-color: #f97316;
}

.icon-box {
  font-size: 2rem;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f8fafc;
  border-radius: 50%;
}

.info h3 {
  margin: 0;
  font-size: 0.85rem;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.info .number {
  margin: 5px 0;
  font-size: 1.8rem;
  font-weight: 700;
  color: #0f172a;
}

.info .detail {
  font-size: 0.8rem;
  color: #94a3b8;
}

/* Contenido Principal */
.content-grid {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1.5rem;
}

.panel {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
}

.panel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.panel h2 {
  margin: 0;
  font-size: 1.2rem;
  color: #334155;
}

.link-btn {
  background: none;
  border: none;
  color: #2563eb;
  cursor: pointer;
  font-weight: 600;
}

/* Tabla */
.table-responsive {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th {
  text-align: left;
  color: #64748b;
  font-size: 0.85rem;
  padding-bottom: 10px;
  border-bottom: 2px solid #f1f5f9;
}

td {
  padding: 12px 0;
  border-bottom: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.95rem;
}

.folio {
  font-family: monospace;
  font-weight: 600;
  color: #64748b;
}

.badge {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.75rem;
  font-weight: 700;
}

.badge.warning {
  background: #fff7ed;
  color: #c2410c;
}

/* Naranja */
.badge.danger {
  background: #fef2f2;
  color: #991b1b;
}

/* Rojo */
.badge.info {
  background: #eff6ff;
  color: #1e40af;
}

/* Azul */

/* Acciones Rápidas */
.quick-actions {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.action-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
  cursor: pointer;
  transition: all 0.2s;
  color: #475569;
  font-weight: 500;
  font-size: 0.9rem;
}

.action-btn:hover {
  background: #f1f5f9;
  transform: translateY(-2px);
  border-color: #cbd5e1;
}

.emoji {
  font-size: 1.5rem;
}

@media (max-width: 768px) {
  .content-grid {
    grid-template-columns: 1fr;
  }
}
</style>