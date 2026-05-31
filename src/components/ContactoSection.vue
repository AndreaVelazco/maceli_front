<template>
  <section id="contacto">
    <div class="contacto-inner">
      <!-- Info -->
      <div class="fade-up">
        <p class="section-label">Contáctanos</p>
        <h2 class="section-title">Hablemos de tu plan ideal</h2>
        <div class="divider"></div>
        <p class="section-sub">Estamos para ayudarte a encontrar la mejor opción. Escríbenos y te respondemos rápido.</p>

        <div style="margin-top: 2.5rem;">
          <div v-for="info in infoItems" :key="info.label" class="contacto-item">
            <span class="contacto-icon">{{ info.icon }}</span>
            <div>
              <strong>{{ info.label }}</strong>
              <span>{{ info.value }}</span>
            </div>
          </div>
        </div>

        <a href="https://wa.me/51999999999?text=Hola%20MACELI%2C%20quiero%20información" target="_blank" class="whatsapp-btn">
          <span>💬</span> Pedir por WhatsApp
        </a>
      </div>

      <!-- Form -->
      <div class="fade-up">
        <div class="contacto-form">
          <div class="form-group">
            <label>Nombre completo</label>
            <input v-model="form.nombre" type="text" placeholder="Tu nombre" />
          </div>
          <div class="form-group">
            <label>Correo electrónico</label>
            <input v-model="form.correo" type="email" placeholder="tu@correo.com" />
          </div>
          <div class="form-group">
            <label>Teléfono / WhatsApp</label>
            <input v-model="form.telefono" type="tel" placeholder="+51 999 999 999" />
          </div>
          <div class="form-group">
            <label>Mensaje</label>
            <textarea v-model="form.mensaje" rows="4" placeholder="Cuéntanos tus objetivos o restricciones alimentarias..."></textarea>
          </div>
          <p v-if="errorMsg" class="error-msg">{{ errorMsg }}</p>
          <button class="form-submit" :disabled="submitting" @click.prevent="handleSubmit">
            {{ submitLabel }}
          </button>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

const API_BASE_URL = 'http://localhost:8080'

const form = ref({ nombre: '', correo: '', telefono: '', mensaje: '' })
const errorMsg  = ref('')
const submitting = ref(false)
const success   = ref(false)

const submitLabel = computed(() => {
  if (submitting.value) return 'Enviando...'
  if (success.value)   return '✓ ¡Mensaje enviado!'
  return 'Enviar mensaje'
})

const infoItems = [
  { icon: '📍', label: 'Ubicación',           value: 'Arequipa, Perú · Dark Kitchen' },
  { icon: '📱', label: 'WhatsApp',             value: '+51 999 999 999' },
  { icon: '📧', label: 'Email',                value: 'hola@maceli.pe' },
  { icon: '🕐', label: 'Horario de atención',  value: 'Lunes a Sábado · 7:00 am – 8:00 pm' },
]

async function handleSubmit() {
  errorMsg.value = ''
  const { nombre, correo, telefono, mensaje } = form.value
  if (!nombre.trim() || !mensaje.trim()) {
    errorMsg.value = 'Por favor completa nombre y mensaje.'
    return
  }
  submitting.value = true
  try {
    const res = await fetch(`${API_BASE_URL}/api/contacto`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nombre, telefono, correo, mensaje }),
    })
    if (!res.ok) {
      const err = await res.json().catch(() => ({}))
      throw new Error(err.error || `HTTP ${res.status}`)
    }
    success.value = true
    form.value = { nombre: '', correo: '', telefono: '', mensaje: '' }
    setTimeout(() => success.value = false, 4000)
  } catch (err) {
    errorMsg.value = `Error: ${err.message}. Intenta por WhatsApp.`
  } finally {
    submitting.value = false
  }
}
</script>

<style scoped>
#contacto { background: var(--dark); }
.contacto-inner {
  max-width: 1100px; margin: 0 auto;
  display: grid; grid-template-columns: 1fr 1fr; gap: 5rem; align-items: start;
}
.contacto-item { display: flex; align-items: flex-start; gap: 1rem; margin-bottom: 1.5rem; }
.contacto-icon { font-size: 1.3rem; margin-top: .1rem; }
.contacto-item strong { display: block; font-size: .82rem; letter-spacing: .1em; text-transform: uppercase; color: var(--accent); margin-bottom: .2rem; }
.contacto-item span { font-size: .9rem; color: var(--mid); }
.whatsapp-btn {
  display: inline-flex; align-items: center; gap: .8rem;
  padding: 1rem 2rem; background: #25D366; color: #fff;
  font-family: 'DM Sans', sans-serif; font-size: .85rem; letter-spacing: .08em;
  text-transform: uppercase; border: none; cursor: pointer;
  text-decoration: none; border-radius: 2px; margin-top: 1.6rem;
  transition: opacity .25s, transform .2s;
}
.whatsapp-btn:hover { opacity: .88; transform: translateY(-2px); }
.contacto-form { display: flex; flex-direction: column; gap: 1.1rem; }
.form-group { display: flex; flex-direction: column; gap: .4rem; }
.form-group label { font-size: .75rem; letter-spacing: .12em; text-transform: uppercase; color: var(--mid); }
.form-group input,
.form-group textarea {
  background: rgba(28,28,28,.7); border: 1px solid rgba(191,191,191,0.18);
  color: var(--light); font-family: 'DM Sans', sans-serif; font-size: .9rem;
  padding: .85rem 1rem; border-radius: 2px; outline: none;
  transition: border-color .25s; resize: none;
}
.form-group input:focus,
.form-group textarea:focus { border-color: var(--accent); }
.error-msg { font-size: .82rem; color: #e57373; }
.form-submit {
  padding: .9rem 2rem; background: var(--accent); color: var(--black);
  font-family: 'DM Sans', sans-serif; font-size: .82rem; letter-spacing: .1em;
  text-transform: uppercase; border: none; cursor: pointer; border-radius: 2px;
  transition: opacity .25s; align-self: flex-start;
}
.form-submit:hover:not(:disabled) { opacity: .88; }
.form-submit:disabled { opacity: .6; cursor: default; }
@media (max-width: 900px) {
  .contacto-inner { grid-template-columns: 1fr; gap: 3rem; }
}
</style>
