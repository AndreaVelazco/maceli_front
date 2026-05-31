<template>
  <section id="planes">
    <div class="planes-header fade-up">
      <p class="section-label">Nuestros planes</p>
      <h2 class="section-title">Elige tu plan saludable</h2>
      <div class="divider"></div>
      <p class="section-sub">Planes flexibles adaptados a tu estilo de vida. Sin complicaciones, con sabor y bienestar garantizado.</p>
    </div>

    <div class="planes-grid fade-up">
      <!-- Skeleton mientras carga -->
      <template v-if="loading">
        <div v-for="n in 3" :key="n" class="plan-card skeleton">
          <div class="plan-icon">⏳</div>
          <div class="plan-name">Cargando...</div>
        </div>
      </template>

      <template v-else-if="planes.length">
        <div
          v-for="plan in planes"
          :key="plan.id"
          class="plan-card"
          :class="{ featured: plan._destacado }"
        >
          <div v-if="plan._destacado" class="plan-badge-top">⭐ Más popular</div>
          <img v-if="plan.imagen_url" :src="plan.imagen_url" :alt="plan.nombre" class="plan-img" />
          <div v-else class="plan-icon">{{ plan._icono || '🍽️' }}</div>
          <div class="plan-name">{{ plan.nombre }}</div>
          <div class="plan-desc">{{ plan.descripcion }}</div>
          <div class="plan-price">S/. {{ Number(plan.precio).toFixed(0) }}</div>
          <div class="plan-price-label">{{ plan._etiqueta || plan.categoria }}</div>
          <ul v-if="plan._beneficios?.length" class="plan-features">
            <li v-for="b in plan._beneficios" :key="b">{{ b }}</li>
          </ul>
          <a :href="`https://wa.me/51999999999?text=${encodeURIComponent('Quiero el ' + plan.nombre + ' de MACELI')}`" target="_blank" class="plan-btn">Pedir ahora</a>
        </div>
      </template>

      <p v-else class="empty-msg">No hay planes disponibles en este momento.</p>
    </div>

    <div class="plan-custom fade-up">
      <div>
        <h3>¿Necesitas algo más específico?</h3>
        <p>Diseñamos un plan 100% a medida según tus objetivos: pérdida de peso, masa muscular, planes especializados o restricciones alimentarias.</p>
      </div>
      <a href="https://wa.me/51999999999?text=Quiero%20un%20plan%20personalizado%20de%20MACELI" target="_blank" class="btn-primary">Consultar plan personalizado</a>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const API_BASE_URL = 'http://localhost:8080'
const loading = ref(true)
const planes  = ref([])

const FALLBACK = [
  {
    id: 1, nombre: 'Plan Diario', descripcion: 'Ideal para probar MACELI sin compromiso. Una jornada completa de comida balanceada.',
    precio: 35, categoria: 'Plan diario', activo: true, imagen_url: '',
    _icono: '🥗', _etiqueta: 'por día · 3 comidas',
    _beneficios: ['Desayuno, almuerzo y cena', 'Ingredientes frescos del día', 'Delivery incluido', 'Sin suscripción']
  },
  {
    id: 2, nombre: 'Plan Semanal', descripcion: 'La opción más elegida. Siete días de alimentación consciente con ahorro garantizado.',
    precio: 200, categoria: 'Plan semanal', activo: true, imagen_url: '', _destacado: true,
    _icono: '🍱', _etiqueta: 'por semana · 3 comidas/día',
    _beneficios: ['21 comidas balanceadas', 'Menú variado sin repetición', 'Delivery diario incluido', 'Asesoría nutricional básica', 'Ajuste por objetivos']
  },
  {
    id: 3, nombre: 'Plan Familiar', descripcion: 'Alimentación saludable para toda la familia. Personalizado por miembro y necesidad.',
    precio: 550, categoria: 'Plan familiar', activo: true, imagen_url: '',
    _icono: '👨‍👩‍👧', _etiqueta: 'por semana · hasta 4 personas',
    _beneficios: ['Hasta 4 personas', 'Menús personalizados por miembro', 'Delivery unificado', 'Asesoría nutricional familiar', 'Ajuste por alergias o restricciones']
  }
]

onMounted(async () => {
  try {
    const res  = await fetch(`${API_BASE_URL}/api/planes`)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    const data = await res.json()
    planes.value = Array.isArray(data) ? data : (data.data || [])
  } catch {
    planes.value = FALLBACK
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
#planes { background: var(--black); }
.planes-header { max-width: 1100px; margin: 0 auto 4rem; }
.planes-grid {
  display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem;
  max-width: 1100px; margin: 0 auto;
}
.plan-card {
  border: 1px solid rgba(191,191,191,0.12);
  border-radius: 4px; padding: 2.5rem 2rem;
  background: var(--dark); position: relative;
  transition: border-color .3s, transform .3s;
  display: flex; flex-direction: column;
}
.plan-card.skeleton { opacity: .4; pointer-events: none; }
.plan-card:hover { border-color: var(--accent); transform: translateY(-6px); }
.plan-card.featured {
  border-color: var(--accent);
  background: linear-gradient(160deg, #3A3A3A 0%, #2a2020 100%);
}
.plan-badge-top {
  position: absolute; top: -1px; left: 50%; transform: translateX(-50%);
  background: var(--accent); color: var(--black);
  font-size: .68rem; letter-spacing: .15em; text-transform: uppercase;
  padding: .3rem 1rem; border-radius: 0 0 3px 3px;
}
.plan-img { width: 100%; height: 140px; object-fit: cover; border-radius: 3px; margin-bottom: 1rem; }
.plan-icon { font-size: 2.4rem; margin-bottom: 1.2rem; }
.plan-name { font-family: 'Cormorant Garamond', serif; font-size: 1.5rem; font-weight: 600; color: var(--light); margin-bottom: .4rem; }
.plan-desc { font-size: .83rem; color: var(--mid); line-height: 1.65; margin-bottom: 1.6rem; }
.plan-price { font-family: 'Cormorant Garamond', serif; font-size: 2.8rem; font-weight: 700; color: var(--accent); line-height: 1; margin-bottom: .3rem; }
.plan-price-label { font-size: .75rem; color: var(--mid); margin-bottom: 1.8rem; }
.plan-features { list-style: none; display: flex; flex-direction: column; gap: .65rem; flex-grow: 1; }
.plan-features li { font-size: .84rem; color: var(--mid); display: flex; align-items: center; gap: .7rem; }
.plan-features li::before { content: '✓'; color: var(--accent); font-weight: 700; }
.plan-btn {
  margin-top: 2rem; padding: .85rem; text-align: center;
  border: 1px solid var(--accent); color: var(--accent);
  background: transparent; font-family: 'DM Sans', sans-serif;
  font-size: .8rem; letter-spacing: .1em; text-transform: uppercase;
  cursor: pointer; text-decoration: none; border-radius: 2px;
  transition: background .25s, color .25s; display: block;
}
.plan-btn:hover, .plan-card.featured .plan-btn { background: var(--accent); color: var(--black); }
.empty-msg { color: var(--mid); text-align: center; grid-column: 1/-1; padding: 2rem; }
.plan-custom {
  max-width: 1100px; margin: 2rem auto 0;
  background: var(--dark); border: 1px solid rgba(217,165,165,0.15);
  border-radius: 4px; padding: 2.5rem;
  display: flex; align-items: center; justify-content: space-between;
  gap: 2rem; flex-wrap: wrap;
}
.plan-custom h3 { font-family: 'Cormorant Garamond', serif; font-size: 1.6rem; color: var(--light); margin-bottom: .4rem; }
.plan-custom p { font-size: .88rem; color: var(--mid); max-width: 500px; }
@media (max-width: 900px) {
  .planes-grid { grid-template-columns: 1fr; }
  .plan-custom { flex-direction: column; }
}
</style>
