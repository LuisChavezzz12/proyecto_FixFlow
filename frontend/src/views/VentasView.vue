<template>
  <div class="page-container">
    
    <header class="page-header">
      <div class="header-left">
        <button class="btn-back" @click="$router.back()" title="Volver">←</button>
        <div>
          <h1>Punto de Venta</h1>
          <p>Registrar venta de productos o servicios.</p>
        </div>
      </div>
      <div class="date-badge">
        {{ currentDate }}
      </div>
    </header>

    <div class="pos-layout">
      
      <div class="catalog-panel">
        
        <div class="search-bar">
          <span class="icon">🔍</span>
          <input 
            type="text" 
            placeholder="Buscar producto por nombre o código..." 
            v-model="searchQuery"
            class="search-input"
          >
        </div>

        <div class="categories">
          <button 
            v-for="cat in categories" 
            :key="cat"
            :class="['cat-pill', { active: activeCategory === cat }]"
            @click="activeCategory = cat"
          >
            {{ cat }}
          </button>
        </div>

        <div class="products-grid">
          <div 
            v-for="product in filteredProducts" 
            :key="product.id" 
            class="product-card"
            @click="addToCart(product)"
          >
            <div class="p-stock" :class="{ 'low': product.stock < 5 }">Stock: {{ product.stock }}</div>
            <div class="p-icon">{{ product.icon }}</div>
            <div class="p-info">
              <h4>{{ product.nombre }}</h4>
              <span class="p-price">${{ product.precio }}</span>
            </div>
          </div>
        </div>
      </div>

      <div class="ticket-panel">
        
        <div class="ticket-header">
          <h3>Ticket de Venta</h3>
          <button class="btn-clear" @click="clearCart" v-if="cart.length > 0">Limpiar</button>
        </div>

        <div class="customer-select">
          <span class="c-icon">👤</span>
          <input type="text" placeholder="Cliente General (Público)" class="c-input">
        </div>

        <div class="cart-items">
          <div v-if="cart.length === 0" class="empty-cart">
            <p>🛒 El carrito está vacío</p>
            <small>Selecciona productos del panel izquierdo</small>
          </div>

          <div v-else class="item-row" v-for="(item, index) in cart" :key="index">
            <div class="item-details">
              <span class="i-name">{{ item.nombre }}</span>
              <span class="i-price">${{ item.precio }}</span>
            </div>
            <div class="item-actions">
              <button class="qty-btn" @click="updateQty(index, -1)">-</button>
              <span class="qty-val">{{ item.qty }}</span>
              <button class="qty-btn" @click="updateQty(index, 1)">+</button>
              <button class="del-btn" @click="removeFromCart(index)">🗑️</button>
            </div>
          </div>
        </div>

        <div class="ticket-footer">
          <div class="summary-row">
            <span>Subtotal:</span>
            <span>${{ subtotal.toFixed(2) }}</span>
          </div>
          <div class="summary-row">
            <span>IVA (16%):</span>
            <span>${{ tax.toFixed(2) }}</span>
          </div>
          <div class="total-row">
            <span>Total:</span>
            <span class="total-amount">${{ total.toFixed(2) }}</span>
          </div>

          <button class="btn-pay" :disabled="cart.length === 0">
            Cobrar Venta
          </button>
        </div>

      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const currentDate = new Date().toLocaleDateString('es-MX', { day: 'numeric', month: 'long', year: 'numeric' });
const searchQuery = ref('');
const activeCategory = ref('Todos');

// Datos Mock (Simulación BD)
const categories = ['Todos', 'Accesorios', 'Refacciones', 'Servicios', 'Micas'];

const products = ref([
  { id: 1, nombre: 'Mica 9D iPhone 11', precio: 150, stock: 45, category: 'Micas', icon: '📱' },
  { id: 2, nombre: 'Cable USB-C 1m', precio: 200, stock: 12, category: 'Accesorios', icon: '🔌' },
  { id: 3, nombre: 'Cubo Carga Rápida', precio: 350, stock: 8, category: 'Accesorios', icon: '⚡' },
  { id: 4, nombre: 'Limpieza General', precio: 250, stock: 999, category: 'Servicios', icon: '🧹' },
  { id: 5, nombre: 'Batería Samsung A50', precio: 650, stock: 3, category: 'Refacciones', icon: '🔋' },
  { id: 6, nombre: 'Funda Silicona Roja', precio: 120, stock: 20, category: 'Accesorios', icon: '🛡️' },
]);

const cart = ref([]);

// Filtros
const filteredProducts = computed(() => {
  return products.value.filter(p => {
    const matchesSearch = p.nombre.toLowerCase().includes(searchQuery.value.toLowerCase());
    const matchesCat = activeCategory.value === 'Todos' || p.category === activeCategory.value;
    return matchesSearch && matchesCat;
  });
});

// Lógica del Carrito
const addToCart = (product) => {
  const existing = cart.value.find(item => item.id === product.id);
  if (existing) {
    existing.qty++;
  } else {
    cart.value.push({ ...product, qty: 1 });
  }
};

const updateQty = (index, amount) => {
  const item = cart.value[index];
  item.qty += amount;
  if (item.qty <= 0) removeFromCart(index);
};

const removeFromCart = (index) => {
  cart.value.splice(index, 1);
};

const clearCart = () => cart.value = [];

// Cálculos
const subtotal = computed(() => cart.value.reduce((acc, item) => acc + (item.precio * item.qty), 0));
const tax = computed(() => subtotal.value * 0.16);
const total = computed(() => subtotal.value + tax.value);

</script>

<style scoped>
.page-container { padding: 2rem; max-width: 1400px; margin: 0 auto; height: 90vh; display: flex; flex-direction: column; }

/* HEADER */
.page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1.5rem; flex-shrink: 0; }
.header-left { display: flex; align-items: center; gap: 1rem; }
.page-header h1 { font-size: 1.8rem; color: #0f172a; margin: 0; font-weight: 800; }
.page-header p { color: #64748b; margin: 2px 0 0; }
.date-badge { color: #64748b; font-weight: 600; background: #f1f5f9; padding: 5px 10px; border-radius: 6px; }

.btn-back { background: white; border: 1px solid #e2e8f0; width: 40px; height: 40px; border-radius: 50%; cursor: pointer; font-size: 1.2rem; color: #64748b; display: flex; align-items: center; justify-content: center; transition: all 0.2s; }
.btn-back:hover { background: #f1f5f9; color: #1e293b; transform: translateX(-2px); }

/* LAYOUT PRINCIPAL */
.pos-layout { display: flex; gap: 1.5rem; flex: 1; overflow: hidden; }

/* --- PANEL IZQUIERDO (CATÁLOGO) --- */
.catalog-panel { flex: 2; display: flex; flex-direction: column; background: white; padding: 1.5rem; border-radius: 12px; border: 1px solid #e2e8f0; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.02); }

/* Buscador */
.search-bar { position: relative; margin-bottom: 1rem; }
.search-bar .icon { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #94a3b8; }
.search-input { width: 100%; padding: 12px 12px 12px 40px; border: 1px solid #cbd5e1; border-radius: 8px; font-size: 1rem; outline: none; background: #f8fafc; }
.search-input:focus { background: white; border-color: #2563eb; box-shadow: 0 0 0 3px rgba(37,99,235,0.1); }

/* Categorías */
.categories { display: flex; gap: 8px; margin-bottom: 1.5rem; overflow-x: auto; padding-bottom: 5px; }
.cat-pill { border: 1px solid #e2e8f0; background: white; padding: 6px 14px; border-radius: 20px; color: #64748b; font-weight: 600; cursor: pointer; white-space: nowrap; transition: all 0.2s; }
.cat-pill:hover { background: #f1f5f9; }
.cat-pill.active { background: #2563eb; color: white; border-color: #2563eb; }

/* Grid Productos */
.products-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 1rem; overflow-y: auto; padding-right: 5px; }
.product-card { border: 1px solid #e2e8f0; border-radius: 10px; padding: 1rem; cursor: pointer; transition: all 0.2s; position: relative; display: flex; flex-direction: column; align-items: center; text-align: center; }
.product-card:hover { border-color: #93c5fd; transform: translateY(-3px); box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); }

.p-stock { position: absolute; top: 8px; right: 8px; font-size: 0.7rem; background: #f1f5f9; padding: 2px 6px; border-radius: 4px; color: #64748b; }
.p-stock.low { color: #ef4444; background: #fef2f2; }
.p-icon { font-size: 2.5rem; margin-bottom: 10px; }
.p-info h4 { font-size: 0.9rem; color: #1e293b; margin: 0 0 5px 0; line-height: 1.2; }
.p-price { font-weight: 700; color: #2563eb; font-size: 1rem; }


/* --- PANEL DERECHO (TICKET) --- */
.ticket-panel { flex: 1; background: white; border-radius: 12px; border: 1px solid #e2e8f0; display: flex; flex-direction: column; overflow: hidden; box-shadow: 0 10px 15px -3px rgba(0,0,0,0.05); }

.ticket-header { padding: 1rem; background: #f8fafc; border-bottom: 1px solid #e2e8f0; display: flex; justify-content: space-between; align-items: center; }
.ticket-header h3 { margin: 0; font-size: 1.1rem; color: #1e293b; }
.btn-clear { background: none; border: none; color: #ef4444; font-size: 0.85rem; font-weight: 600; cursor: pointer; }
.btn-clear:hover { text-decoration: underline; }

.customer-select { padding: 1rem; display: flex; align-items: center; gap: 8px; border-bottom: 1px dashed #e2e8f0; }
.c-input { border: none; background: #f1f5f9; padding: 8px; border-radius: 6px; flex: 1; outline: none; font-size: 0.9rem; }
.c-input:focus { background: white; box-shadow: 0 0 0 2px #bfdbfe; }

.cart-items { flex: 1; overflow-y: auto; padding: 1rem; }
.empty-cart { text-align: center; color: #94a3b8; margin-top: 2rem; }
.empty-cart p { font-size: 1.2rem; font-weight: 600; margin-bottom: 5px; }

.item-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem; border-bottom: 1px solid #f8fafc; padding-bottom: 0.5rem; }
.item-details { flex: 1; }
.i-name { display: block; font-size: 0.9rem; font-weight: 600; color: #334155; }
.i-price { font-size: 0.85rem; color: #64748b; }

.item-actions { display: flex; align-items: center; gap: 5px; }
.qty-btn { width: 24px; height: 24px; border: 1px solid #e2e8f0; background: white; border-radius: 4px; cursor: pointer; display: flex; align-items: center; justify-content: center; color: #64748b; }
.qty-btn:hover { background: #f1f5f9; color: #0f172a; }
.qty-val { font-size: 0.9rem; font-weight: 600; min-width: 20px; text-align: center; }
.del-btn { background: none; border: none; cursor: pointer; font-size: 1rem; margin-left: 5px; opacity: 0.5; }
.del-btn:hover { opacity: 1; }

.ticket-footer { padding: 1.5rem; background: #f8fafc; border-top: 1px solid #e2e8f0; }
.summary-row { display: flex; justify-content: space-between; font-size: 0.9rem; color: #64748b; margin-bottom: 5px; }
.total-row { display: flex; justify-content: space-between; font-size: 1.4rem; font-weight: 800; color: #0f172a; margin-top: 10px; margin-bottom: 1rem; padding-top: 10px; border-top: 1px dashed #cbd5e1; }

.btn-pay { width: 100%; background: #16a34a; color: white; border: none; padding: 14px; border-radius: 8px; font-size: 1.1rem; font-weight: 700; cursor: pointer; transition: all 0.2s; }
.btn-pay:hover:not(:disabled) { background: #15803d; transform: translateY(-2px); box-shadow: 0 4px 10px rgba(22, 163, 74, 0.3); }
.btn-pay:disabled { background: #cbd5e1; cursor: not-allowed; }

@media (max-width: 900px) {
  .pos-layout { flex-direction: column; overflow-y: auto; }
  .catalog-panel { height: 400px; flex: none; }
  .page-container { height: auto; }
}
</style>