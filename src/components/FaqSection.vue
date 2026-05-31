<template>
  <section id="faq">
    <div class="faq-inner">
      <div class="faq-header fade-up">
        <p class="section-label">Preguntas frecuentes</p>
        <h2 class="section-title">Todo lo que necesitas saber</h2>
        <div class="divider"></div>
      </div>

      <div v-for="(item, i) in faqs" :key="i" class="faq-item fade-up" :class="{ open: openIndex === i }">
        <button class="faq-q" @click="toggle(i)">
          {{ item.q }}
          <span class="faq-icon">+</span>
        </button>
        <div class="faq-a">{{ item.a }}</div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const openIndex = ref(null)
const toggle = (i) => openIndex.value = openIndex.value === i ? null : i

const faqs = [
  { q: '¿Cómo funciona el servicio de delivery?', a: 'Realizamos entregas diarias según el plan contratado. Coordinamos el horario y punto de entrega contigo al momento de confirmar tu pedido. Cubrimos las principales zonas de Arequipa.' },
  { q: '¿Puedo cambiar mi plan una vez contratado?', a: 'Sí. Entendemos que las necesidades cambian. Puedes ajustar tu plan comunicándote con nosotros por WhatsApp con 24 horas de anticipación.' },
  { q: '¿Trabajan con alergias o restricciones alimentarias?', a: 'Absolutamente. Indicamos todos los alérgenos en nuestros menús y podemos adaptar las preparaciones a restricciones como intolerancia al gluten, lactosa, dietas vegetarianas o veganas, entre otras.' },
  { q: '¿Cómo se realiza el pago?', a: 'Aceptamos pagos por transferencia bancaria, Yape, Plin y efectivo. Los planes son prepagados para garantizar la frescura y organización de cada entrega.' },
  { q: '¿Con cuánta anticipación debo hacer mi pedido?', a: 'Recomendamos coordinar con al menos 24 horas de anticipación para garantizar disponibilidad y la mejor preparación de tus comidas.' },
  { q: '¿Los planes incluyen asesoría nutricional?', a: 'Los planes semanales, familiares y personalizados incluyen orientación nutricional básica. Para planes especializados (pérdida de peso, aumento de masa muscular), ofrecemos asesoramiento más detallado.' },
]
</script>

<style scoped>
#faq { background: var(--black); }
.faq-inner { max-width: 760px; margin: 0 auto; }
.faq-header { margin-bottom: 3rem; }
.faq-item { border-bottom: 1px solid rgba(191,191,191,0.12); overflow: hidden; }
.faq-q {
  display: flex; align-items: center; justify-content: space-between;
  padding: 1.4rem 0; cursor: pointer;
  font-size: .96rem; color: var(--light);
  background: none; border: none; width: 100%; text-align: left;
  font-family: 'DM Sans', sans-serif; transition: color .25s;
}
.faq-q:hover { color: var(--accent); }
.faq-icon { font-size: 1.3rem; color: var(--accent); transition: transform .3s; flex-shrink: 0; }
.faq-item.open .faq-icon { transform: rotate(45deg); }
.faq-a {
  max-height: 0; overflow: hidden;
  font-size: .88rem; color: var(--mid); line-height: 1.75;
  transition: max-height .4s ease, padding .3s;
}
.faq-item.open .faq-a { max-height: 200px; padding-bottom: 1.2rem; }
</style>
