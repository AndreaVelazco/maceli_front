<template>
  <NavBar />
  <HeroSection />
  <SobreSection />
  <PlanesSection />
  <BeneficiosSection />
  <FaqSection />
  <ContactoSection />
  <FooterBar />
</template>

<script setup>
import { onMounted } from 'vue'
import NavBar            from './components/NavBar.vue'
import HeroSection       from './components/HeroSection.vue'
import SobreSection      from './components/SobreSection.vue'
import PlanesSection     from './components/PlanesSection.vue'
import BeneficiosSection from './components/BeneficiosSection.vue'
import FaqSection        from './components/FaqSection.vue'
import ContactoSection   from './components/ContactoSection.vue'
import FooterBar         from './components/FooterBar.vue'

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible') }),
    { threshold: 0.12 }
  )
  document.querySelectorAll('.fade-up').forEach(el => observer.observe(el))
})
</script>
