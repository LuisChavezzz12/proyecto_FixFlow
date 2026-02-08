<template>
    <div class="page-container">

        <header class="module-header">
            <div class="header-left">
                <button class="btn-back" @click="$router.back()" title="Volver">
                    ←
                </button>
                <div class="header-text">
                    <h1>Inventario</h1>
                    <p>Gestiona refacciones y productos.</p>
                </div>
            </div>

            <button class="btn-primary">
                <span class="icon">+</span> Nuevo Producto
            </button>
        </header>

        <div class="toolbar">
            <div class="search-box">
                <span class="search-icon">🔍</span>
                <input type="text" placeholder="Buscar por nombre, código o categoría..." v-model="searchQuery">
            </div>

            <div class="filters">
                <button v-for="filter in filters" :key="filter.key"
                    :class="['filter-tab', { active: activeFilter === filter.key }]" @click="activeFilter = filter.key">
                    {{ filter.label }}
                </button>
            </div>
        </div>

        <div class="table-container">
            <table class="inventory-table">
                <thead>
                    <tr>
                        <th>Producto</th>
                        <th>Categoría</th>
                        <th>Precio Venta</th>
                        <th>Costo</th>
                        <th>Stock</th>
                        <th>Estado</th>
                        <th class="text-right">Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in filteredItems" :key="item.id">
                        <td>
                            <div class="product-info">
                                <span class="product-name">{{ item.nombre }}</span>
                                <span class="product-code">SKU: {{ item.sku }}</span>
                            </div>
                        </td>
                        <td><span class="category-tag">{{ item.categoria }}</span></td>
                        <td class="font-bold">${{ item.precio }}</td>
                        <td class="text-muted">${{ item.costo }}</td>
                        <td>
                            <div class="stock-wrapper">
                                <span class="stock-qty">{{ item.stock }}</span>
                                <span class="stock-bar">
                                    <span class="fill"
                                        :style="{ width: getStockPercentage(item) + '%', background: getStockColor(item) }"></span>
                                </span>
                            </div>
                        </td>
                        <td>
                            <span :class="['status-badge', getStockStatusClass(item)]">
                                {{ getStockLabel(item) }}
                            </span>
                        </td>
                        <td class="text-right">
                            <button class="btn-icon edit">✏️</button>
                            <button class="btn-icon delete">🗑️</button>
                        </td>
                    </tr>
                </tbody>
            </table>

            <div v-if="filteredItems.length === 0" class="empty-state">
                <p>No se encontraron productos con "{{ searchQuery }}"</p>
            </div>
        </div>

    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const searchQuery = ref('');
const activeFilter = ref('all');

const filters = [
    { key: 'all', label: 'Todos' },
    { key: 'low', label: '⚠️ Bajo Stock' },
    { key: 'out', label: '⛔ Agotados' }
];

const inventory = ref([
    { id: 1, nombre: 'Pantalla iPhone 11', sku: 'SCR-IP11-ORIG', categoria: 'Pantallas', precio: 1200, costo: 600, stock: 12, min_stock: 5 },
    { id: 2, nombre: 'Batería Samsung A51', sku: 'BAT-SAM-A51', categoria: 'Baterías', precio: 450, costo: 150, stock: 3, min_stock: 5 },
    { id: 3, nombre: 'Centro de Carga Tipo C', sku: 'CHR-USBC-GEN', categoria: 'Soldadura', precio: 150, costo: 20, stock: 0, min_stock: 10 },
    { id: 4, nombre: 'Mica Cristal Templado 9D', sku: 'GLASS-UNIV', categoria: 'Accesorios', precio: 100, costo: 15, stock: 45, min_stock: 10 },
    { id: 5, nombre: 'Pasta Térmica Artic', sku: 'TOOL-PASTE', categoria: 'Insumos', precio: 300, costo: 180, stock: 2, min_stock: 3 },
]);

const filteredItems = computed(() => {
    let items = inventory.value;
    if (searchQuery.value) {
        const q = searchQuery.value.toLowerCase();
        items = items.filter(i => i.nombre.toLowerCase().includes(q) || i.sku.toLowerCase().includes(q));
    }
    if (activeFilter.value === 'low') items = items.filter(i => i.stock > 0 && i.stock <= i.min_stock);
    else if (activeFilter.value === 'out') items = items.filter(i => i.stock === 0);
    return items;
});

const getStockPercentage = (item) => Math.min((item.stock / (item.min_stock * 3)) * 100, 100);
const getStockColor = (item) => {
    if (item.stock === 0) return '#ef4444';
    if (item.stock <= item.min_stock) return '#f59e0b';
    return '#10b981';
};
const getStockStatusClass = (item) => {
    if (item.stock === 0) return 'danger';
    if (item.stock <= item.min_stock) return 'warning';
    return 'success';
};
const getStockLabel = (item) => {
    if (item.stock === 0) return 'Agotado';
    if (item.stock <= item.min_stock) return 'Bajo';
    return 'Normal';
};
</script>

<style scoped>
.page-container {
    padding: 2rem;
    max-width: 1400px;
    margin: 0 auto;
}

/* HEADER CON BOTÓN VOLVER */
.module-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
}

.header-left {
    display: flex;
    align-items: center;
    gap: 1rem;
}

/* Estilos del Botón Volver */
.btn-back {
    background: white;
    border: 1px solid #e2e8f0;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.2rem;
    color: #64748b;
    transition: all 0.2s;
}

.btn-back:hover {
    background: #f1f5f9;
    color: #1e293b;
    border-color: #cbd5e1;
    transform: translateX(-2px);
}

.module-header h1 {
    font-size: 1.8rem;
    color: #0f172a;
    margin: 0;
    font-weight: 800;
}

.module-header p {
    color: #64748b;
    margin: 2px 0 0;
    font-size: 0.9rem;
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
    transition: background 0.2s;
}

.btn-primary:hover {
    background: #1d4ed8;
}

/* Toolbar */
.toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
    gap: 20px;
    flex-wrap: wrap;
}

.search-box {
    position: relative;
    flex: 1;
    max-width: 400px;
}

.search-icon {
    position: absolute;
    left: 12px;
    top: 50%;
    transform: translateY(-50%);
    color: #94a3b8;
}

.search-box input {
    width: 100%;
    padding: 10px 10px 10px 38px;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    font-size: 0.95rem;
    outline: none;
}

.search-box input:focus {
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

.filters {
    display: flex;
    gap: 5px;
    background: #f1f5f9;
    padding: 4px;
    border-radius: 8px;
}

.filter-tab {
    border: none;
    background: none;
    padding: 6px 14px;
    border-radius: 6px;
    font-size: 0.85rem;
    font-weight: 600;
    color: #64748b;
    cursor: pointer;
    transition: all 0.2s;
}

.filter-tab:hover {
    color: #1e293b;
    background: rgba(255, 255, 255, 0.5);
}

.filter-tab.active {
    background: white;
    color: #2563eb;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

/* Tabla */
.table-container {
    background: white;
    border-radius: 12px;
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
    overflow: hidden;
    border: 1px solid #e2e8f0;
}

.inventory-table {
    width: 100%;
    border-collapse: collapse;
}

.inventory-table th {
    text-align: left;
    padding: 1rem;
    background: #f8fafc;
    color: #64748b;
    font-size: 0.75rem;
    font-weight: 700;
    text-transform: uppercase;
    border-bottom: 1px solid #e2e8f0;
}

.inventory-table td {
    padding: 1rem;
    border-bottom: 1px solid #f1f5f9;
    color: #334155;
    vertical-align: middle;
}

.inventory-table tr:hover td {
    background: #fcfcfc;
}

/* Elementos Tabla */
.product-info {
    display: flex;
    flex-direction: column;
}

.product-name {
    font-weight: 600;
    color: #0f172a;
}

.product-code {
    font-size: 0.75rem;
    color: #94a3b8;
    margin-top: 2px;
}

.category-tag {
    background: #f1f5f9;
    padding: 4px 8px;
    border-radius: 6px;
    font-size: 0.8rem;
    color: #475569;
    font-weight: 500;
}

.font-bold {
    font-weight: 700;
    color: #0f172a;
}

.text-muted {
    color: #94a3b8;
    font-size: 0.9rem;
    text-decoration: line-through;
    opacity: 0.5;
}

.stock-wrapper {
    width: 100px;
}

.stock-qty {
    font-weight: 700;
    font-size: 0.9rem;
    margin-bottom: 4px;
    display: block;
}

.stock-bar {
    display: block;
    height: 6px;
    background: #e2e8f0;
    border-radius: 3px;
    overflow: hidden;
}

.stock-bar .fill {
    display: block;
    height: 100%;
    border-radius: 3px;
    transition: width 0.3s ease;
}

.status-badge {
    padding: 4px 10px;
    border-radius: 20px;
    font-size: 0.75rem;
    font-weight: 700;
    display: inline-block;
}

.status-badge.success {
    background: #dcfce7;
    color: #166534;
}

.status-badge.warning {
    background: #fef3c7;
    color: #b45309;
}

.status-badge.danger {
    background: #fee2e2;
    color: #991b1b;
}

.text-right {
    text-align: right;
}

.btn-icon {
    background: none;
    border: none;
    cursor: pointer;
    font-size: 1.1rem;
    padding: 5px;
    opacity: 0.6;
    transition: all 0.2s;
}

.btn-icon:hover {
    opacity: 1;
    background: #f1f5f9;
    border-radius: 6px;
}

.btn-icon.edit:hover {
    color: #2563eb;
}

.btn-icon.delete:hover {
    color: #ef4444;
}

.empty-state {
    padding: 3rem;
    text-align: center;
    color: #94a3b8;
}
</style>