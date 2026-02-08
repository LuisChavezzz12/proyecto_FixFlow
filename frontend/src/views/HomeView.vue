<template>
  <div class="dashboard-container">

    <header class="dashboard-header">
      <div>
        <h1>Panel de Control</h1>
        <p class="date-text">Hoy es {{ currentDate }}</p>
      </div>
      <button class="btn-primary" @click="$router.push('/home/recepcion')">
        <span class="icon">+</span> Nueva Orden
      </button>
    </header>

    <div class="modules-grid">
      <div class="module-card action-blue" @click="$router.push('/recepcion')">
        <span class="module-icon">🛎️</span>
        <div class="module-info">
          <h3>Recepción</h3>
          <p>Ingresar equipos</p>
        </div>
      </div>
      <div class="module-card action-orange" @click="$router.push('/inventario')">
        <span class="module-icon">📦</span>
        <div class="module-info">
          <h3>Inventario</h3>
          <p>Consultar stock</p>
        </div>
      </div>
      <div class="module-card action-green" @click="$router.push('/ventas')">
        <span class="module-icon">💵</span>
        <div class="module-info">
          <h3>Caja</h3>
          <p>Registrar venta</p>
        </div>
      </div>
      <div class="module-card action-purple" @click="$router.push('/taller')">
        <span class="module-icon">🔧</span>
        <div class="module-info">
          <h3>Taller</h3>
          <p>Ver reparaciones</p>
        </div>
      </div>
    </div>

    <div class="toggle-bar" @click="showStats = !showStats" :class="{ 'active': showStats }">
      <span class="toggle-text">
        {{ showStats ? 'Ocultar Indicadores' : 'Ver Estadísticas del Día' }}
      </span>
      <span class="toggle-icon">{{ showStats ? '▲' : '▼' }}</span>
    </div>

    <Transition name="slide-fade">
      <div v-if="showStats" class="stats-overview">
        <div class="stat-box">
          <span class="stat-title">En Taller</span>
          <span class="stat-value">12</span>
          <span class="stat-trend neutral">Activos</span>
        </div>
        <div class="stat-box">
          <span class="stat-title">Por Entregar</span>
          <span class="stat-value text-green">5</span>
          <span class="stat-trend positive">Listos</span>
        </div>
        <div class="stat-box">
          <span class="stat-title">Ventas Hoy</span>
          <span class="stat-value">$4,250</span>
          <span class="stat-trend positive">↗ 12%</span>
        </div>
        <div class="stat-box">
          <span class="stat-title">Alertas Stock</span>
          <span class="stat-value text-red">3</span>
          <span class="stat-trend negative">Críticos</span>
        </div>
      </div>
    </Transition>

    <div class="main-section">
      <div class="section-header">
        <h2>⚠️ Atención Requerida</h2>
        <button class="link-btn" @click="$router.push('/taller')">Ver todo →</button>
      </div>

      <div class="card-table">
        <table class="clean-table">
          <thead>
            <tr>
              <th>Folio</th>
              <th>Cliente</th>
              <th>Equipo</th>
              <th>Ingreso</th>
              <th>Estado</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="orden in urgentOrders" :key="orden.id">
              <td class="font-mono">#{{ orden.folio }}</td>
              <td class="font-bold">{{ orden.cliente }}</td>
              <td>{{ orden.equipo }}</td>
              <td class="text-muted">{{ orden.fecha }}</td>
              <td>
                <span :class="['status-badge', orden.statusClass]">{{ orden.status }}</span>
              </td>
              <td>
                <button class="btn-icon">✏️</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';

const currentDate = new Date().toLocaleDateString('es-MX', { weekday: 'long', day: 'numeric', month: 'long' });
const showStats = ref(false);

const urgentOrders = ref([
  { id: 1, folio: '00154', cliente: 'Roberto Gómez', equipo: 'iPhone 11 Pro', fecha: 'Hace 2 días', status: 'Diagnóstico', statusClass: 'warning' },
  { id: 2, folio: '00152', cliente: 'Ana K. López', equipo: 'MacBook Air M1', fecha: 'Hace 3 días', status: 'Reparación', statusClass: 'info' },
  { id: 3, folio: '00149', cliente: 'Hotel Rosales', equipo: 'Impresora Epson', fecha: 'Hace 5 días', status: 'Retrasado', statusClass: 'danger' },
]);
</script>

<style scoped>
.dashboard-container {
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
}

/* HEADER */
.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.dashboard-header h1 {
  font-size: 1.8rem;
  color: #0f172a;
  margin: 0;
  font-weight: 800;
}

.date-text {
  color: #64748b;
  margin: 5px 0 0;
  font-size: 0.95rem;
  text-transform: capitalize;
}

.btn-primary {
  background: #2563eb;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.2);
  transition: all 0.2s;
}

.btn-primary:hover {
  background: #1d4ed8;
  transform: translateY(-2px);
}

/* MODULES GRID */
.modules-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-bottom: 1.5rem;
}

.module-card {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  display: flex;
  align-items: center;
  gap: 1rem;
  cursor: pointer;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.module-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05);
}

/* Colores Módulos */
.action-blue:hover {
  background: #eff6ff;
  border-color: #bfdbfe;
}

.action-orange:hover {
  background: #fff7ed;
  border-color: #fed7aa;
}

.action-green:hover {
  background: #f0fdf4;
  border-color: #bbf7d0;
}

.action-purple:hover {
  background: #faf5ff;
  border-color: #e9d5ff;
}

.module-icon {
  font-size: 2rem;
}

.module-info h3 {
  margin: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #1e293b;
}

.module-info p {
  margin: 2px 0 0;
  font-size: 0.8rem;
  color: #64748b;
}

/* --- NUEVO ESTILO: BARRA TOGGLE CÓMODA --- */
.toggle-bar {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  background-color: #f8fafc;
  /* Gris muy suave */
  border: 1px dashed #cbd5e1;
  /* Borde punteado sutil */
  border-radius: 8px;
  padding: 10px;
  margin-bottom: 1.5rem;
  cursor: pointer;
  color: #64748b;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.2s;
  user-select: none;
  /* Evita que se seleccione el texto al hacer clic rápido */
}

.toggle-bar:hover {
  background-color: #f1f5f9;
  color: #2563eb;
  /* Azul al pasar el mouse */
  border-color: #93c5fd;
  border-style: solid;
  /* Cambia a sólido */
}

.toggle-bar.active {
  background-color: #eff6ff;
  /* Azul muy claro cuando está abierto */
  color: #2563eb;
  border-style: solid;
  border-color: #bfdbfe;
  margin-bottom: 1.5rem;
}

.toggle-icon {
  font-size: 0.8rem;
  transition: transform 0.3s;
}

/* STATS */
.stats-overview {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1.5rem;
  margin-bottom: 2rem;
}

@media (max-width: 900px) {
  .stats-overview {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 500px) {
  .stats-overview {
    grid-template-columns: 1fr;
  }
}

.stat-box {
  background: white;
  padding: 1.2rem;
  border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  border: 1px solid #f1f5f9;
  text-align: center;
}

.stat-title {
  display: block;
  font-size: 0.75rem;
  color: #64748b;
  font-weight: 700;
  text-transform: uppercase;
  margin-bottom: 5px;
}

.stat-value {
  display: block;
  font-size: 1.8rem;
  font-weight: 800;
  color: #0f172a;
}

.stat-trend {
  font-size: 0.8rem;
  font-weight: 500;
}

.text-green {
  color: #16a34a;
}

.text-red {
  color: #dc2626;
}

.positive {
  color: #16a34a;
}

.negative {
  color: #dc2626;
}

.neutral {
  color: #64748b;
}

/* TABLE SECTION */
.main-section {
  margin-top: 1rem;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.section-header h2 {
  font-size: 1.2rem;
  color: #1e293b;
  margin: 0;
  font-weight: 700;
}

.link-btn {
  background: none;
  border: none;
  color: #2563eb;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
}

.link-btn:hover {
  text-decoration: underline;
}

.card-table {
  background: white;
  border-radius: 12px;
  overflow-x: auto;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02);
  border: 1px solid #f1f5f9;
}

.clean-table {
  width: 100%;
  border-collapse: collapse;
  min-width: 600px;
}

/* Min-width para scroll horizontal en móvil */
.clean-table th {
  text-align: left;
  padding: 1rem;
  background: #f8fafc;
  color: #64748b;
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.clean-table td {
  padding: 1rem;
  border-bottom: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.9rem;
}

.clean-table tr:last-child td {
  border-bottom: none;
}

.clean-table tr:hover td {
  background: #fcfcfc;
}

.status-badge {
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 700;
  display: inline-block;
}

.status-badge.warning {
  background: #fff7ed;
  color: #ea580c;
  border: 1px solid #ffedd5;
}

.status-badge.info {
  background: #eff6ff;
  color: #2563eb;
  border: 1px solid #dbeafe;
}

.status-badge.danger {
  background: #fef2f2;
  color: #dc2626;
  border: 1px solid #fee2e2;
}

.btn-icon {
  background: none;
  border: none;
  cursor: pointer;
  opacity: 0.4;
  transition: all 0.2s;
  font-size: 1.1rem;
}

.btn-icon:hover {
  opacity: 1;
  transform: scale(1.1);
}

.font-mono {
  font-family: 'Courier New', monospace;
  color: #64748b;
  font-weight: 600;
}

.font-bold {
  font-weight: 600;
  color: #0f172a;
}

.text-muted {
  color: #94a3b8;
  font-size: 0.85rem;
}

/* ANIMACIONES */
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}
</style>