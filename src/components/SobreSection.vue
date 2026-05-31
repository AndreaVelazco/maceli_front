<template>
  <section id="sobre">
    <div class="sobre-grid">
      <div class="fade-up">
        <p class="section-label">Quiénes somos</p>
        <h2 class="section-title">Alimentación que transforma vidas</h2>
        <div class="divider"></div>
        <p class="section-sub">En MACELI – Eat Fit rompemos el estigma de que lo saludable es aburrido. Somos una dark kitchen especializada en comidas balanceadas, nutritivas y con sabor real.</p>
        <div class="valores-list">
          <div v-for="valor in valores" :key="valor" class="valor-item">
            <span class="valor-dot"></span>{{ valor }}
          </div>
        </div>
      </div>
      <div class="sobre-visual fade-up">
        <div v-for="(card, i) in cards" :key="card.title" class="sobre-card" :class="{ offset: i === 1 || i === 3 }">
          <div class="sobre-icon">{{ card.icon }}</div>
          <h4>{{ card.title }}</h4>
          <p>{{ card.text }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const valores = [
  'Honestidad en cada ingrediente y proceso',
  'Innovación continua en recetas y servicios',
  'Puntualidad garantizada en cada entrega',
  'Cercanía y trato personalizado',
  'Compromiso con la sostenibilidad',
]

const cards = [
  { icon: '🎯', title: 'Misión',  text: 'Comida saludable y deliciosa de forma práctica, dejando una huella de sabor, calidad y bienestar.' },
  { icon: '🚀', title: 'Visión',  text: 'Ser la marca líder en comida saludable en Perú, inspirando una nueva generación de hábitos conscientes.' },
  { icon: '🥦', title: 'Nutrición personalizada', text: 'Planes diseñados según tus objetivos con asesoramiento nutricional especializado.' },
  { icon: '🛵', title: 'Delivery puntual', text: 'Tu comida lista y fresca, entregada a tiempo donde la necesites.' },
]
</script>

<style scoped>
#sobre { background: var(--dark); }
.sobre-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 5rem; align-items: center;
  max-width: 1100px; margin: 0 auto;
}
.sobre-visual { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
.sobre-card {
  background: var(--black); border-radius: 4px;
  padding: 1.8rem 1.4rem; border: 1px solid rgba(217,165,165,0.1);
  transition: border-color .3s, transform .3s;
}
.sobre-card:hover { border-color: var(--accent); transform: translateY(-4px); }
.sobre-card.offset { margin-top: 2rem; }
.sobre-icon { font-size: 2rem; margin-bottom: 1rem; }
.sobre-card h4 {
  font-family: 'Cormorant Garamond', serif;
  font-size: 1.15rem; color: var(--light); margin-bottom: .4rem;
}
.sobre-card p { font-size: .83rem; color: var(--mid); line-height: 1.65; }
.valores-list { margin-top: 2rem; display: flex; flex-direction: column; gap: .7rem; }
.valor-item { display: flex; align-items: center; gap: .8rem; font-size: .88rem; color: var(--mid); }
.valor-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); flex-shrink: 0; }
@media (max-width: 900px) {
  .sobre-grid { grid-template-columns: 1fr; gap: 3rem; }
  .sobre-visual { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 560px) {
  .sobre-visual { grid-template-columns: 1fr; }
  .sobre-card.offset { margin-top: 0; }
}
</style>
