<script setup>
import { ref, onMounted } from 'vue'
import { Code2, Smartphone, Server, Layout } from 'lucide-vue-next'

const sectionRef = ref(null)

const highlights = [
  { icon: Code2,      label: 'Web',      value: 'Angular & Vue.js' },
  { icon: Smartphone, label: 'Mobile',   value: 'Flutter iOS & Android' },
  { icon: Server,     label: 'Back-end', value: 'Spring Boot & Laravel' },
  { icon: Layout,     label: 'Produit',  value: 'Figma → production' },
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
      <div class="about__intro-row">
        <div class="section-header reveal">
          <span class="section-label">01 — À propos</span>
          <h2>Du front au back, jusqu’au mobile.</h2>
        </div>
        <p class="about__lead reveal">
          Je m’appelle <strong>Néhémie Pédahel Kouyo</strong>. Développeur full-stack basé à Abidjan,
          je conçois des produits web et mobiles de bout en bout — interfaces, APIs, données —
          pour des contextes métier exigeants : paiement, santé, logistique, communication.
        </p>
      </div>

      <div class="about__grid">
        <div class="about__text reveal-left">
          <p>
            Basé à <strong>Abidjan</strong>, je travaille actuellement chez
            <strong>Kyrmann Software Engineering</strong> en tant que développeur full-stack :
            plateformes Angular en production, APIs REST, intégration Figma, et livraison en sprints Scrum.
          </p>
          <p>
            Avant cela : apps Flutter publiées sur les stores, microservices
            <strong>Spring Boot / Docker / PostgreSQL</strong>, et produits
            <strong>Vue.js / Laravel / PHP / MySQL</strong>.
            La stack change, le fil reste le même : clarté, performance, maintenabilité.
          </p>

          <div class="about__meta">
            <div>
              <span>Disponibilité</span>
              <strong class="available"><i></i> Ouvert aux échanges</strong>
            </div>
            <div>
              <span>Localisation</span>
              <strong>Abidjan, CI</strong>
            </div>
            <div>
              <span>Langue</span>
              <strong>Français</strong>
            </div>
          </div>
        </div>

        <div class="about__side reveal-right">
          <div
            v-for="item in highlights"
            :key="item.label"
            class="about__card"
          >
            <div class="about__card-icon">
              <component :is="item.icon" :size="20" />
            </div>
            <div>
              <small>{{ item.label }}</small>
              <strong>{{ item.value }}</strong>
            </div>
          </div>

          <div class="about__stats">
            <div>
              <b>4+</b>
              <span>Années d’expérience</span>
            </div>
            <div>
              <b>5+</b>
              <span>Produits livrés</span>
            </div>
            <div>
              <b>2</b>
              <span>Stores mobiles</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.about {
  background: $bg-secondary;
  border-top: 1px solid $border-color;

  &__intro-row {
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    gap: 40px;
    align-items: end;
    margin-bottom: 56px;

    .section-header { margin-bottom: 0; }

    @media (max-width: 800px) {
      grid-template-columns: 1fr;
    }
  }

  &__lead {
    font-size: 1.12rem;
    color: $text-secondary;
    line-height: 1.75;
    padding-bottom: 8px;

    strong { color: $text-primary; }
  }

  &__grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 56px;

    @media (max-width: 900px) {
      grid-template-columns: 1fr;
      gap: 40px;
    }
  }

  &__text p {
    color: $text-secondary;
    margin-bottom: 18px;
    font-size: 0.98rem;
    line-height: 1.85;

    strong { color: $text-primary; font-weight: 600; }
  }

  &__meta {
    display: flex;
    flex-wrap: wrap;
    gap: 28px;
    margin-top: 32px;
    padding-top: 28px;
    border-top: 1px solid $border-color;

    span {
      display: block;
      font-family: $font-mono;
      font-size: 0.68rem;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      color: $text-muted;
      margin-bottom: 6px;
    }

    strong {
      font-size: 0.95rem;
      font-weight: 650;
    }

    .available {
      color: $status-ok;
      display: inline-flex;
      align-items: center;
      gap: 8px;

      i {
        width: 8px;
        height: 8px;
        border-radius: 50%;
        background: $status-ok;
      }
    }
  }

  &__side {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  &__card {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px 18px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    transition: $transition-base;

    &:hover {
      border-color: rgba($accent, 0.4);
      transform: translateX(4px);
    }

    small {
      display: block;
      font-family: $font-mono;
      font-size: 0.68rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: $text-muted;
    }

    strong {
      font-size: 0.95rem;
      font-weight: 650;
    }
  }

  &__card-icon {
    width: 42px;
    height: 42px;
    border-radius: 10px;
    display: grid;
    place-items: center;
    color: $accent;
    background: rgba($accent, 0.1);
    border: 1px solid rgba($accent, 0.16);
    flex-shrink: 0;
  }

  &__stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-top: 8px;

    div {
      padding: 20px 10px;
      text-align: center;
      background: $bg-card;
      border: 1px solid $border-color;
      border-radius: $radius-md;
    }

    b {
      display: block;
      font-family: $font-heading;
      font-size: 1.7rem;
      font-weight: 800;
      color: $accent-soft;
      letter-spacing: -0.04em;
    }

    span {
      font-size: 0.72rem;
      color: $text-muted;
      line-height: 1.35;
    }
  }
}
</style>
