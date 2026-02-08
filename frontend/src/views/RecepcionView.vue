<template>
  <div class="page-container">
    
    <header class="page-header">
      <div class="header-left">
        <button class="btn-back" @click="$router.back()" title="Volver">
          ←
        </button>
        <div>
          <h1>Nueva Orden de Servicio</h1>
          <p>Ingresa los datos para recibir un equipo.</p>
        </div>
      </div>
      
      <div class="folio-badge">
        Folio Sugerido: <strong>#00155</strong>
      </div>
    </header>

    <div class="reception-grid">
      
      <div class="card-section">
        <div class="section-title">
          <span class="icon-circle">👤</span>
          <h3>Datos del Cliente</h3>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label>Nombre Completo</label>
            <input type="text" placeholder="Buscar o escribir nombre..." class="input-field">
          </div>
          <div class="form-group">
            <label>Teléfono / WhatsApp</label>
            <input type="tel" placeholder="Ej. 55 1234 5678" class="input-field">
          </div>
        </div>
        
        <div class="form-group">
          <label>Correo Electrónico (Opcional)</label>
          <input type="email" placeholder="cliente@email.com" class="input-field">
        </div>
      </div>

      <div class="card-section">
        <div class="section-title">
          <span class="icon-circle">📱</span>
          <h3>Datos del Equipo</h3>
        </div>

        <div class="form-row">
          <div class="form-group">
            <label>Tipo de Equipo</label>
            <select class="input-field">
              <option>Smartphone</option>
              <option>Laptop</option>
              <option>Tablet</option>
              <option>Consola</option>
              <option>Otro</option>
            </select>
          </div>
          <div class="form-group">
            <label>Marca</label>
            <input type="text" placeholder="Ej. Samsung, Apple" class="input-field">
          </div>
        </div>

        <div class="form-row">
          <div class="form-group">
            <label>Modelo</label>
            <input type="text" placeholder="Ej. Galaxy S20" class="input-field">
          </div>
          <div class="form-group">
            <label>Contraseña / Patrón</label>
            <input type="text" placeholder="Sin bloqueo" class="input-field">
          </div>
        </div>
      </div>

      <div class="card-section full-width">
        <div class="section-title">
          <span class="icon-circle">🔧</span>
          <h3>Diagnóstico Inicial</h3>
        </div>

        <div class="form-group">
          <label>Falla Reportada</label>
          <textarea rows="3" placeholder="Describe el problema que menciona el cliente..." class="input-field"></textarea>
        </div>

        <div class="form-row">
          <div class="form-group">
            <label>Accesorios (Cargador, Funda, etc.)</label>
            <input type="text" placeholder="Solo equipo" class="input-field">
          </div>
          <div class="form-group">
            <label>Cotización Estimada ($)</label>
            <input type="number" placeholder="0.00" class="input-field money-input">
          </div>
        </div>
        
        <div class="checklist-group">
          <label class="checkbox-label">
            <input type="checkbox"> Pantalla rota
          </label>
          <label class="checkbox-label">
            <input type="checkbox"> No enciende
          </label>
          <label class="checkbox-label">
            <input type="checkbox"> Mojado
          </label>
          <label class="checkbox-label">
            <input type="checkbox"> Tapa trasera dañada
          </label>
        </div>

      </div>

    </div>

    <div class="action-footer">
      <button class="btn-cancel" @click="$router.back()">Cancelar</button>
      <button class="btn-save">Generar Orden e Imprimir</button>
    </div>

  </div>
</template>

<style scoped>
.page-container { padding: 2rem; max-width: 1200px; margin: 0 auto; }

/* HEADER */
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem; }

/* NUEVO: Agrupador Izquierdo para alinear botón y textos */
.header-left { display: flex; align-items: center; gap: 1rem; }

.page-header h1 { font-size: 1.8rem; color: #0f172a; margin: 0; font-weight: 800; }
.page-header p { color: #64748b; margin: 5px 0 0; }
.folio-badge { background: #eff6ff; color: #2563eb; padding: 8px 16px; border-radius: 8px; font-size: 0.9rem; border: 1px solid #bfdbfe; }

/* BOTÓN VOLVER (Estilo Inventario) */
.btn-back {
  background: white; border: 1px solid #e2e8f0; width: 40px; height: 40px;
  border-radius: 50%; cursor: pointer; display: flex; align-items: center; justify-content: center;
  font-size: 1.2rem; color: #64748b; transition: all 0.2s;
}
.btn-back:hover { background: #f1f5f9; color: #1e293b; border-color: #cbd5e1; transform: translateX(-2px); }

/* GRID LAYOUT */
.reception-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-bottom: 2rem;
}
.full-width { grid-column: span 2; }

@media (max-width: 768px) {
  .reception-grid { grid-template-columns: 1fr; }
  .full-width { grid-column: span 1; }
}

/* CARDS */
.card-section { background: white; padding: 1.5rem; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); border: 1px solid #e2e8f0; }

.section-title { display: flex; align-items: center; gap: 10px; margin-bottom: 1.5rem; border-bottom: 1px solid #f1f5f9; padding-bottom: 10px; }
.section-title h3 { margin: 0; font-size: 1.1rem; color: #1e293b; font-weight: 700; }
.icon-circle { width: 32px; height: 32px; background: #f8fafc; border-radius: 50%; display: flex; justify-content: center; align-items: center; font-size: 1.1rem; }

/* FORMULARIOS */
.form-row { display: flex; gap: 1.5rem; margin-bottom: 1rem; }
.form-group { flex: 1; margin-bottom: 1rem; }
label { display: block; font-size: 0.85rem; font-weight: 600; color: #475569; margin-bottom: 6px; }

.input-field {
  width: 100%; padding: 10px 12px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 0.95rem; outline: none; transition: all 0.2s; background: #f8fafc;
}
.input-field:focus { background: white; border-color: #2563eb; box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1); }
textarea.input-field { resize: vertical; }

.money-input { font-weight: bold; color: #166534; }

/* CHECKLIST */
.checklist-group { display: flex; gap: 15px; flex-wrap: wrap; margin-top: 10px; }
.checkbox-label { display: flex; align-items: center; gap: 6px; font-size: 0.9rem; color: #334155; cursor: pointer; }
.checkbox-label input { accent-color: #2563eb; width: 16px; height: 16px; }

/* FOOTER */
.action-footer { display: flex; justify-content: flex-end; gap: 1rem; margin-top: 1rem; padding-top: 1rem; border-top: 1px solid #e2e8f0; }
.btn-cancel { background: white; border: 1px solid #cbd5e1; color: #64748b; padding: 12px 24px; border-radius: 8px; font-weight: 600; cursor: pointer; }
.btn-cancel:hover { background: #f1f5f9; color: #1e293b; }

.btn-save { background: #2563eb; color: white; border: none; padding: 12px 30px; border-radius: 8px; font-weight: 600; cursor: pointer; box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.2); }
.btn-save:hover { background: #1d4ed8; transform: translateY(-1px); }
</style>