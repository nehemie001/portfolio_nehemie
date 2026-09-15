<script setup>
import { ref, onMounted } from 'vue'
import { Calendar, MapPin } from 'lucide-vue-next'

const sectionRef = ref(null)

const experiences = [
  {
    id: 1,
    role: 'Développeur Web Angular',
    company: 'Kyrmann Software Engineering',
    location: 'Abidjan, CI',
    period: 'Mai 2025 – Aujourd\'hui',
    type: 'CDI',
    tasks: [
      'Développement des plateformes PRESTIGE-SMS (campagnes SMS) et PRIMUS KARE (parcours de soins)',
      'Intégration de maquettes Figma en composants Angular réutilisables',
      'Fonctionnalités front-end en collaboration produit, suivi de tickets Jira / Scrum',
      'Performance, accessibilité et qualité des interfaces métier',
    ],
    tags: ['Angular', 'TypeScript', 'Figma', 'Jira', 'Scrum', 'RxJS'],
  },
  {
    id: 2,
    role: 'Développeur Mobile Flutter',
    company: 'Eburnie Hub',
    location: 'Abidjan, CI',
    period: 'Janvier 2025 – Mai 2025',
    type: 'Stage',
    tasks: [
      'Application mobile éducative iOS et Android',
      'Widgets Flutter réutilisables, état avec Bloc / Provider',
      'Intégration d’APIs REST, tests, publication App Store et Play Store',
    ],
    tags: ['Flutter', 'Dart', 'Bloc', 'Provider', 'iOS', 'Android'],
  },
  {
    id: 3,
    role: 'Développeur Full-Stack Spring Boot / Angular',
    company: 'Atos CI',
    location: 'Abidjan, CI',
    period: 'Août 2024 – Novembre 2024',
    type: 'Stage',
    tasks: [
      'Microservices Spring Boot et Docker',
      'Application de gestion scolaire (inscriptions, notes, emplois du temps)',
      'Module santé avec vidéoconférence, PostgreSQL et documentation Swagger',
    ],
    tags: ['Spring Boot', 'Angular', 'Docker', 'PostgreSQL', 'Swagger'],
  },
  {
    id: 4,
    role: 'Développeur Web Laravel / Vue.js',
    company: 'DigitAfrikGroup',
    location: 'Abidjan, CI',
    period: 'Mars 2021 – Décembre 2023',
    type: 'CDI',
    tasks: [
      'Interfaces Vue.js / Pinia et APIs Laravel',
      'CRUD, authentification, rôles, mise en production',
      'Maintenance et optimisation d’applications métier',
    ],
    tags: ['Laravel', 'Vue.js', 'Pinia', 'PHP', 'MySQL', 'WordPress'],
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .exp__item').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="experience" class="section experience" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">03 — Expérience</span>
        <h2>Parcours professionnel</h2>
        <p>Des contextes variés : éditeur logiciel, hub d’innovation, ESN, agence digitale.</p>
      </div>

      <div class="exp">
        <article
          v-for="(exp, i) in experiences"
          :key="exp.id"
          class="exp__item"
          :style="`--delay: ${i * 0.1}s`"
        >
          <div class="exp__meta">
            <span class="exp__index">0{{ i + 1 }}</span>
            <span class="exp__type">{{ exp.type }}</span>
            <span class="exp__period"><Calendar :size="13" /> {{ exp.period }}</span>
            <span class="exp__loc"><MapPin :size="13" /> {{ exp.location }}</span>
          </div>

          <div class="exp__body">
            <h3>{{ exp.role }}</h3>
            <p class="exp__company">{{ exp.company }}</p>
            <ul>
              <li v-for="task in exp.tasks" :key="task">{{ task }}</li>
            </ul>
            <div class="exp__tags">
              <span v-for="tag in exp.tags" :key="tag">{{ tag }}</span>
            </div>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.experience {
  background: $bg-secondary;
  border-top: 1px solid $border-color;
}

.exp {
  display: flex;
  flex-direction: column;

  &__item {
    display: grid;
    grid-template-columns: 220px 1fr;
    gap: 32px;
    padding: 36px 0;
    border-top: 1px solid $border-color;
    opacity: 0;
    transform: translateY(16px);
    transition: opacity 0.55s ease, transform 0.55s ease;
    transition-delay: var(--delay);

    &.visible {
      opacity: 1;
      transform: none;
    }

    &:last-child { border-bottom: 1px solid $border-color; }

    @media (max-width: 760px) {
      grid-template-columns: 1fr;
      gap: 16px;
      padding: 28px 0;
    }
  }

  &__meta {
    display: flex;
    flex-direction: column;
    gap: 8px;
    padding-top: 4px;
  }

  &__index {
    font-family: $font-heading;
    font-size: 1.4rem;
    font-weight: 800;
    color: $accent;
    letter-spacing: -0.04em;
  }

  &__type {
    align-self: flex-start;
    font-family: $font-mono;
    font-size: 0.68rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: $accent-soft;
    border: 1px solid rgba($accent, 0.3);
    padding: 3px 8px;
    border-radius: 100px;
  }

  &__period,
  &__loc {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.8rem;
    color: $text-muted;
  }

  &__body {
    h3 {
      font-size: 1.2rem;
      margin-bottom: 4px;
    }
  }

  &__company {
    color: $accent;
    font-weight: 600;
    margin-bottom: 16px;
  }

  ul {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 18px;
  }

  li {
    position: relative;
    padding-left: 16px;
    font-size: 0.92rem;
    color: $text-secondary;
    line-height: 1.65;

    &::before {
      content: '';
      position: absolute;
      left: 0;
      top: 0.55em;
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: $accent;
    }
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;

    span {
      font-family: $font-mono;
      font-size: 0.7rem;
      padding: 4px 10px;
      border-radius: 100px;
      background: rgba($accent, 0.08);
      color: $accent-soft;
      border: 1px solid rgba($accent, 0.16);
    }
  }
}
</style>
