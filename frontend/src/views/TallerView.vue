<template>
  <div class="page-container">

    <header class="page-header">
      <div class="header-left">
        <button class="btn-back" @click="$router.back()" title="Volver">←</button>
        <div>
          <h1>Control de Taller</h1>
          <p>Gestiona el flujo de las reparaciones.</p>
        </div>
      </div>

      <div class="technician-filter">
        <span class="filter-label">Técnico:</span>
        <select v-model="selectedTech" class="tech-select">
          <option value="all">Todos</option>
          <option value="Juan">Juan Pérez</option>
          <option value="Pedro">Pedro M.</option>
        </select>
      </div>
    </header>

    <div class="kanban-board">

      <div class="kanban-column">
        <div class="column-header header-pending">
          <h3>Por Revisar</h3>
          <span class="count-badge">{{ getOrdersByStatus('pending').length }}</span>
        </div>
        <div class="column-body">
          <div v-for="order in getOrdersByStatus('pending')" :key="order.id" class="kanban-card">
            <div class="card-top">
              <span class="folio">#{{ order.folio }}</span>
              <span :class="['priority-dot', order.priority]"></span>
            </div>
            <h4>{{ order.device }}</h4>
            <p class="issue">"{{ order.issue }}"</p>
            <div class="card-footer">
              <span class="client-name">👤 {{ order.client }}</span>
              <button class="btn-move" @click="moveOrder(order, 'working')">Iniciar ➡</button>
            </div>
          </div>
        </div>
      </div>

      <div class="kanban-column">
        <div class="column-header header-working">
          <h3>En Reparación</h3>
          <span class="count-badge">{{ getOrdersByStatus('working').length }}</span>
        </div>
        <div class="column-body">
          <div v-for="order in getOrdersByStatus('working')" :key="order.id" class="kanban-card">
            <div class="card-top">
              <span class="folio">#{{ order.folio }}</span>
              <span class="tech-badge">🛠️ {{ order.tech }}</span>
            </div>
            <h4>{{ order.device }}</h4>
            <p class="issue">{{ order.issue }}</p>

            <div class="card-actions">
              <button class="action-btn wait" @click="moveOrder(order, 'waiting')" title="Poner en espera">⏸</button>
              <button class="action-btn finish" @click="moveOrder(order, 'finished')" title="Terminar">✅</button>
            </div>
          </div>
        </div>
      </div>

      <div class="kanban-column">
        <div class="column-header header-waiting">
          <h3>En Espera</h3>
          <span class="count-badge">{{ getOrdersByStatus('waiting').length }}</span>
        </div>
        <div class="column-body">
          <div v-for="order in getOrdersByStatus('waiting')" :key="order.id" class="kanban-card">
            <div class="card-top">
              <span class="folio">#{{ order.folio }}</span>
              <span class="reason-badge">⚠️ {{ order.reason }}</span>
            </div>
            <h4>{{ order.device }}</h4>
            <div class="card-footer">
              <span class="time-elapsed">Hace 2 días</span>
              <button class="btn-move" @click="moveOrder(order, 'working')">Reanudar ⬅</button>
            </div>
          </div>
        </div>
      </div>

      <div class="kanban-column">
        <div class="column-header header-finished">
          <h3>Listos</h3>
          <span class="count-badge">{{ getOrdersByStatus('finished').length }}</span>
        </div>
        <div class="column-body">
          <div v-for="order in getOrdersByStatus('finished')" :key="order.id" class="kanban-card">
            <div class="card-top">
              <span class="folio">#{{ order.folio }}</span>
              <span class="price-badge">${{ order.price }}</span>
            </div>
            <h4>{{ order.device }}</h4>
            <p class="client-info">✅ Cliente Notificado</p>
            <button class="btn-deliver">Entregar</button>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
// ... (El script sigue exactamente igual) ...
import { ref, computed } from 'vue';

const selectedTech = ref('all');

const orders = ref([
  { id: 1, folio: '0155', device: 'iPhone 11 Pro', client: 'Roberto G.', issue: 'Pantalla rota', status: 'pending', priority: 'high', tech: null },
  { id: 2, folio: '0154', device: 'HP Pavilion', client: 'Ana López', issue: 'Lento, virus', status: 'working', priority: 'normal', tech: 'Juan' },
  { id: 3, folio: '0150', device: 'Nintendo Switch', client: 'Carlos M.', issue: 'No carga', status: 'waiting', reason: 'Pieza', priority: 'normal', tech: 'Pedro' },
  { id: 4, folio: '0148', device: 'Samsung A51', client: 'Maria F.', issue: 'Cambio batería', status: 'finished', price: '850', priority: 'low', tech: 'Juan' },
  { id: 5, folio: '0156', device: 'MacBook Air', client: 'Empresa ACME', issue: 'No enciende', status: 'pending', priority: 'urgent', tech: null },
]);

const getOrdersByStatus = (status) => {
  return orders.value.filter(o => {
    const statusMatch = o.status === status;
    const techMatch = selectedTech.value === 'all' || o.tech === selectedTech.value;
    if (status === 'pending') return statusMatch;
    return statusMatch && techMatch;
  });
};

const moveOrder = (order, newStatus) => {
  order.status = newStatus;
  if (newStatus === 'working' && !order.tech) {
    order.tech = 'Yo (Admin)';
  }
};
</script>

<style scoped>
/* ... (Estilos del Header iguales) ... */
.page-container {
  padding: 2rem;
  max-width: 100%;
  margin: 0 auto;
  height: 90vh;
  display: flex;
  flex-direction: column;
}

.page-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
  flex-shrink: 0;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.page-header h1 {
  font-size: 1.8rem;
  color: #0f172a;
  margin: 0;
  font-weight: 800;
}

.page-header p {
  color: #64748b;
  margin: 2px 0 0;
}

.btn-back {
  background: white;
  border: 1px solid #e2e8f0;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.2rem;
  color: #64748b;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.btn-back:hover {
  background: #f1f5f9;
  color: #1e293b;
  transform: translateX(-2px);
}

.technician-filter {
  display: flex;
  align-items: center;
  gap: 10px;
  background: white;
  padding: 5px 10px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.filter-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #64748b;
}

.tech-select {
  border: none;
  outline: none;
  font-size: 0.9rem;
  font-weight: 600;
  color: #0f172a;
  cursor: pointer;
}

/* KANBAN BOARD - DISEÑO MÁS SOBRIO */
.kanban-board {
  flex: 1;
  display: flex;
  gap: 1.5rem;
  overflow-x: auto;
  padding-bottom: 1rem;
}

.kanban-column {
  flex: 1;
  min-width: 280px;
  background: #f8fafc;
  /* Fondo gris muy claro */
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  height: 100%;
  border: 1px solid #e2e8f0;
}

/* Header de Columna - Limpio */
.column-header {
  padding: 1rem;
  background: white;
  /* Fondo blanco */
  border-radius: 12px 12px 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #e2e8f0;
}

.column-header h3 {
  margin: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #334155;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.count-badge {
  background: #f1f5f9;
  color: #64748b;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: 700;
  border: 1px solid #e2e8f0;
}

/* Acentos de color sutiles en el borde superior */
.header-pending {
  border-top: 3px solid #64748b;
}

.header-working {
  border-top: 3px solid #3b82f6;
}

.header-waiting {
  border-top: 3px solid #f59e0b;
}

.header-finished {
  border-top: 3px solid #10b981;
}

.column-body {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

/* TARJETAS - SIN ANIMACIONES BRUSCAS */
.kanban-card {
  background: white;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  border: 1px solid #e2e8f0;
  transition: box-shadow 0.2s;
  /* Solo transición de sombra */
  position: relative;
  /* Eliminado cursor: grab y transform en hover */
}

.kanban-card:hover {
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  border-color: #cbd5e1;
}

/* Se eliminaron los bordes izquierdos gruesos (.active-card, etc.) para un look más limpio */

.card-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.folio {
  font-family: monospace;
  font-size: 0.8rem;
  color: #94a3b8;
  font-weight: 700;
}

.priority-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: block;
}

.priority-dot.urgent {
  background: #ef4444;
}

.priority-dot.high {
  background: #f97316;
}

.priority-dot.normal {
  background: #3b82f6;
}

.priority-dot.low {
  background: #10b981;
}

.kanban-card h4 {
  margin: 0 0 5px 0;
  font-size: 0.95rem;
  color: #0f172a;
  font-weight: 700;
}

.issue {
  margin: 0 0 10px 0;
  font-size: 0.85rem;
  color: #64748b;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: auto;
  padding-top: 8px;
  border-top: 1px solid #f1f5f9;
}

.client-name {
  font-size: 0.75rem;
  color: #64748b;
  font-weight: 500;
}

/* Botones de acción más sutiles */
.btn-move {
  background: none;
  border: none;
  font-size: 0.75rem;
  font-weight: 700;
  color: #3b82f6;
  cursor: pointer;
  padding: 4px 8px;
  border-radius: 4px;
  transition: background 0.2s;
}

.btn-move:hover {
  background: #eff6ff;
}

.tech-badge {
  font-size: 0.75rem;
  background: #f1f5f9;
  color: #475569;
  padding: 2px 6px;
  border-radius: 4px;
  border: 1px solid #e2e8f0;
}

.reason-badge {
  font-size: 0.75rem;
  background: #fff7ed;
  color: #c2410c;
  padding: 2px 6px;
  border-radius: 4px;
  font-weight: 600;
  border: 1px solid #ffedd5;
}

.price-badge {
  font-weight: 700;
  color: #15803d;
  background: #dcfce7;
  padding: 2px 6px;
  border-radius: 4px;
  border: 1px solid #bbf7d0;
}

.card-actions {
  display: flex;
  gap: 5px;
  margin-top: 10px;
  justify-content: flex-end;
}

.action-btn {
  width: 28px;
  height: 28px;
  border-radius: 4px;
  border: 1px solid #e2e8f0;
  background: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.9rem;
  color: #64748b;
  transition: all 0.2s;
}

/* Eliminado transform scale */
.action-btn:hover {
  background: #f1f5f9;
  color: #0f172a;
  border-color: #cbd5e1;
}

.action-btn.wait:hover {
  color: #d97706;
  background: #fff7ed;
  border-color: #fed7aa;
}

.action-btn.finish:hover {
  color: #16a34a;
  background: #f0fdf4;
  border-color: #bbf7d0;
}

.btn-deliver {
  width: 100%;
  margin-top: 10px;
  background: #10b981;
  color: white;
  border: none;
  padding: 8px;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.btn-deliver:hover {
  background: #059669;
}

.client-info {
  font-size: 0.8rem;
  color: #64748b;
  margin-bottom: 0;
}
</style>