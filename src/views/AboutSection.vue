<script setup>
import { ref, onMounted } from 'vue'
import { Code2, Smartphone, Server, Users } from 'lucide-vue-next'

const sectionRef = ref(null)

const highlights = [
  { icon: Code2,     label: 'Web',      value: 'Angular & Vue.js',    color: '#6366f1' },
  { icon: Smartphone,label: 'Mobile',   value: 'Flutter iOS & Android', color: '#8b5cf6' },
  { icon: Server,    label: 'Back-end', value: 'Spring Boot & Laravel',  color: '#22d3ee' },
  { icon: Users,     label: 'Equipe',   value: 'Agile & Leadership',     color: '#ec4899' },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.15 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .reveal-left, .reveal-right').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="about" class="section about" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">// À propos</span>
        <h2>Qui suis-je ?</h2>
        <p>Développeur passionné, je transforme des idées complexes en solutions digitales simples et percutantes.</p>
      </div>

      <div class="about__grid">
        <!-- Text side -->
        <div class="about__text reveal-left">
          <p class="about__intro">
            Je m'appelle <strong>Néhémie Pédahel Kouyo</strong>, développeur web et mobile
            basé à <strong>Abidjan, Côte d'Ivoire</strong>. Avec plusieurs années d'expérience
            dans des environnements exigeants et variés, j'ai eu l'occasion de concevoir des
            applications complètes, de la stratégie UI/UX jusqu'au déploiement en production.
          </p>
          <p class="about__body">
            Que ce soit pour développer une application mobile Flutter publiée sur l'App Store
            et le Play Store, construire une architecture microservices robuste avec Spring Boot
            et Docker, ou créer des interfaces web réactives avec Angular ou Vue.js, je m'investis
            pleinement dans chaque projet avec rigueur et créativité.
          </p>
          <p class="about__body">
            Animé par la curiosité et le goût du défi, je reste constamment à l'affût des
            dernières évolutions technologiques pour offrir des solutions modernes, maintenables
            et performantes.
          </p>

          <div class="about__meta">
            <div class="about__meta-item">
              <span class="about__meta-key">Disponibilité</span>
              <span class="about__meta-val available"><span class="dot"></span>Disponible</span>
            </div>
            <div class="about__meta-item">
              <span class="about__meta-key">Localisation</span>
              <span class="about__meta-val">Abidjan, CI</span>
            </div>
            <div class="about__meta-item">
              <span class="about__meta-key">Langues</span>
              <span class="about__meta-val">Français</span>
            </div>
          </div>
        </div>

        <!-- Highlight cards -->
        <div class="about__cards reveal-right">
          <div
            v-for="(item, i) in highlights"
            :key="item.label"
            class="about__card"
            :style="`--card-color: ${item.color}; --delay: ${i * 0.1}s`"
          >
            <div class="about__card-icon">
              <component :is="item.icon" :size="22" />
            </div>
            <div class="about__card-text">
              <span class="about__card-label">{{ item.label }}</span>
              <span class="about__card-val">{{ item.value }}</span>
            </div>
          </div>

          <!-- Stats -->
          <div class="about__stats">
            <div class="about__stat">
              <span class="about__stat-num gradient-text">4+</span>
              <span class="about__stat-label">Ans d'expérience</span>
            </div>
            <div class="about__stat">
              <span class="about__stat-num gradient-text">10+</span>
              <span class="about__stat-label">Technologies maîtrisées</span>
            </div>
            <div class="about__stat">
              <span class="about__stat-num gradient-text">3+</span>
              <span class="about__stat-label">Projets déployés</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.about {
  background: $bg-secondary;

  &__grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 64px;
    align-items: start;

    @media (max-width: 900px) {
      grid-template-columns: 1fr;
      gap: 48px;
    }
  }

  &__intro {
    font-size: 1.1rem;
    font-weight: 500;
    color: $text-primary;
    line-height: 1.8;
    margin-bottom: 20px;

    strong { color: $accent-blue; font-weight: 700; }
  }

  &__body {
    color: $text-secondary;
    line-height: 1.9;
    margin-bottom: 20px;
    font-size: 0.97rem;
  }

  &__meta {
    display: flex;
    gap: 28px;
    flex-wrap: wrap;
    margin-top: 32px;
    padding-top: 28px;
    border-top: 1px solid $border-color;
  }

  &__meta-item {
    display: flex;
    flex-direction: column;
    gap: 4px;
  }

  &__meta-key {
    font-size: 0.75rem;
    font-weight: 500;
    color: $text-muted;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    font-family: $font-mono;
  }

  &__meta-val {
    font-size: 0.95rem;
    font-weight: 600;
    color: $text-primary;

    &.available {
      color: #22c55e;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .dot {
      width: 8px; height: 8px;
      border-radius: 50%;
      background: #22c55e;
      animation: pulseGreen 2s infinite;
    }
  }

  // ── Cards ────────────────────────────────────────────────
  &__cards {
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  &__card {
    display: flex;
    align-items: center;
    gap: 18px;
    padding: 18px 22px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    transition: $transition-base;
    transition-delay: var(--delay);

    &:hover {
      border-color: var(--card-color, $accent-blue);
      background: $bg-card-hover;
      transform: translateX(6px);
      box-shadow: -4px 0 24px rgba(0,0,0,0.2), 4px 0 0 var(--card-color, $accent-blue) inset;
    }
  }

  &__card-icon {
    flex-shrink: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 46px; height: 46px;
    border-radius: $radius-sm;
    background: rgba(var(--card-color, #6366f1), 0.1);
    color: var(--card-color, $accent-blue);
    background: linear-gradient(135deg, rgba(99,102,241,0.15), rgba(139,92,246,0.15));
    color: var(--card-color);
    border: 1px solid rgba(255,255,255,0.06);
  }

  &__card-text {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  &__card-label {
    font-size: 0.72rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: $text-muted;
    font-family: $font-mono;
  }

  &__card-val {
    font-size: 0.95rem;
    font-weight: 600;
    color: $text-primary;
  }

  // ── Stats ─────────────────────────────────────────────────
  &__stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-top: 8px;
  }

  &__stat {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
    padding: 22px 12px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    text-align: center;
    transition: $transition-base;

    &:hover {
      border-color: $border-hover;
      background: $bg-card-hover;
    }
  }

  &__stat-num {
    font-size: 1.8rem;
    font-weight: 800;
    font-family: $font-heading;
  }

  &__stat-label {
    font-size: 0.72rem;
    color: $text-muted;
    font-weight: 500;
    text-align: center;
    line-height: 1.4;
  }
}

@keyframes pulseGreen {
  0%, 100% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.5); }
  70%       { box-shadow: 0 0 0 6px rgba(34, 197, 94, 0); }
}
</style>
