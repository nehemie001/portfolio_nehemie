<script setup>
import { ref, onMounted } from 'vue'
import { Briefcase, Calendar, MapPin, ChevronRight } from 'lucide-vue-next'

const sectionRef = ref(null)

const experiences = [
  {
    id: 1,
    role: 'Développeur Web Angular',
    company: 'Kyrmann Software Engineering',
    location: 'Abidjan, CI',
    period: 'Mai 2025 – Aujourd\'hui',
    type: 'CDI',
    color: '#6366f1',
    tasks: [
      'Intégration de maquettes Figma en composants Angular réutilisables et maintenables',
      'Développement de fonctionnalités front-end en collaboration avec les équipes produit',
      'Gestion des sprints et des tickets via Jira en méthodologie agile Scrum',
      'Optimisation des performances et de l\'accessibilité des interfaces web',
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
    color: '#8b5cf6',
    tasks: [
      'Développement d\'une application mobile éducative pour iOS et Android',
      'Conception et implémentation de widgets Flutter réutilisables et personnalisables',
      'Intégration d\'APIs REST et gestion de l\'état avec Bloc/Provider',
      'Tests unitaires et d\'intégration, publication sur App Store et Play Store',
    ],
    tags: ['Flutter', 'Dart', 'Bloc', 'Provider', 'iOS', 'Android', 'API REST'],
  },
  {
    id: 3,
    role: 'Développeur Full-Stack Spring Boot / Angular',
    company: 'Atos CI',
    location: 'Abidjan, CI',
    period: 'Août 2024 – Novembre 2024',
    type: 'Stage',
    color: '#22d3ee',
    tasks: [
      'Architecture et développement de microservices avec Spring Boot et Docker',
      'Développement d\'une application de gestion scolaire (inscriptions, notes, emplois du temps)',
      'Application de santé avec module de vidéoconférence pour consultations à distance',
      'Base de données PostgreSQL, documentation API avec Swagger',
    ],
    tags: ['Spring Boot', 'Angular', 'Docker', 'PostgreSQL', 'Microservices', 'Swagger'],
  },
  {
    id: 4,
    role: 'Développeur Web Laravel / Vue.js',
    company: 'DigitAfrikGroup',
    location: 'Abidjan, CI',
    period: 'Mars 2021 – Décembre 2023',
    type: 'CDI',
    color: '#ec4899',
    tasks: [
      'Conception et développement d\'interfaces web modernes avec Vue.js et Pinia',
      'Développement d\'APIs REST avec Laravel pour diverses applications métier',
      'Implémentation de fonctionnalités CRUD, authentification et gestion des rôles',
      'Maintenance, optimisation et mise en production des applications',
    ],
    tags: ['Laravel', 'Vue.js', 'Pinia', 'PHP', 'MySQL', 'API REST', 'WordPress'],
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
        <span class="section-label">// Expériences</span>
        <h2>Parcours professionnel</h2>
        <p>Des expériences variées qui m'ont forgé en développeur polyvalent et rigoureux.</p>
      </div>

      <div class="exp__timeline">
        <!-- Vertical line -->
        <div class="exp__line"></div>

        <div
          v-for="(exp, i) in experiences"
          :key="exp.id"
          class="exp__item"
          :style="`--exp-color: ${exp.color}; --delay: ${i * 0.15}s`"
        >
          <!-- Dot -->
          <div class="exp__dot">
            <Briefcase :size="16" />
          </div>

          <!-- Card -->
          <div class="exp__card">
            <!-- Top meta -->
            <div class="exp__card-top">
              <div class="exp__card-left">
                <span class="exp__type-badge" :style="`color: ${exp.color}; border-color: ${exp.color}30; background: ${exp.color}10`">
                  {{ exp.type }}
                </span>
                <h3 class="exp__role">{{ exp.role }}</h3>
                <p class="exp__company">{{ exp.company }}</p>
              </div>
              <div class="exp__card-right">
                <div class="exp__meta-item">
                  <Calendar :size="14" />
                  <span>{{ exp.period }}</span>
                </div>
                <div class="exp__meta-item">
                  <MapPin :size="14" />
                  <span>{{ exp.location }}</span>
                </div>
              </div>
            </div>

            <!-- Tasks -->
            <ul class="exp__tasks">
              <li v-for="task in exp.tasks" :key="task">
                <ChevronRight :size="14" :style="`color: ${exp.color}`" />
                {{ task }}
              </li>
            </ul>

            <!-- Tags -->
            <div class="exp__tags">
              <span
                v-for="tag in exp.tags"
                :key="tag"
                class="exp__tag"
                :style="`color: ${exp.color}; border-color: ${exp.color}25; background: ${exp.color}0d`"
              >{{ tag }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.experience {
  background: $bg-secondary;
}

.exp {
  &__timeline {
    position: relative;
    padding-left: 50px;

    @media (max-width: 640px) {
      padding-left: 32px;
    }
  }

  &__line {
    position: absolute;
    left: 19px;
    top: 0;
    bottom: 0;
    width: 2px;
    background: linear-gradient(to bottom, $accent-blue, $accent-violet, transparent);
    border-radius: 1px;

    @media (max-width: 640px) {
      left: 11px;
    }
  }

  &__item {
    position: relative;
    margin-bottom: 40px;
    opacity: 0;
    transform: translateX(-20px);
    transition: opacity 0.6s ease, transform 0.6s ease;
    transition-delay: var(--delay);

    &.visible {
      opacity: 1;
      transform: translateX(0);
    }

    &:last-child { margin-bottom: 0; }
  }

  &__dot {
    position: absolute;
    left: -42px;
    top: 24px;
    width: 40px; height: 40px;
    border-radius: 50%;
    background: $bg-card;
    border: 2px solid var(--exp-color, $accent-blue);
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--exp-color, $accent-blue);
    box-shadow: 0 0 0 4px rgba(0,0,0,0.4), 0 0 16px var(--exp-color, $accent-blue);
    z-index: 1;
    transition: $transition-base;

    @media (max-width: 640px) {
      left: -28px;
      width: 28px; height: 28px;
      top: 28px;

      svg { display: none; }
    }
  }

  &__card {
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    padding: 28px 28px 22px;
    transition: $transition-base;

    &:hover {
      border-color: var(--exp-color, $accent-blue);
      background: $bg-card-hover;
      box-shadow: 0 8px 32px rgba(0,0,0,0.3), 0 0 0 1px var(--exp-color) inset;
      transform: translateY(-2px);
    }

    @media (max-width: 640px) {
      padding: 20px 18px 16px;
    }
  }

  &__card-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 24px;
    margin-bottom: 20px;

    @media (max-width: 700px) {
      flex-direction: column;
      gap: 12px;
    }
  }

  &__type-badge {
    display: inline-block;
    font-size: 0.72rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    padding: 3px 10px;
    border-radius: 100px;
    border: 1px solid;
    margin-bottom: 10px;
    font-family: $font-mono;
  }

  &__role {
    font-size: 1.08rem;
    font-weight: 700;
    color: $text-primary;
    margin-bottom: 4px;
  }

  &__company {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--exp-color, $accent-blue);
  }

  &__card-right {
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex-shrink: 0;
    text-align: right;

    @media (max-width: 700px) {
      text-align: left;
    }
  }

  &__meta-item {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.82rem;
    color: $text-muted;
    justify-content: flex-end;

    @media (max-width: 700px) {
      justify-content: flex-start;
    }

    svg { flex-shrink: 0; }
  }

  &__tasks {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 20px;

    li {
      display: flex;
      align-items: flex-start;
      gap: 8px;
      font-size: 0.9rem;
      color: $text-secondary;
      line-height: 1.6;

      svg { flex-shrink: 0; margin-top: 3px; }
    }
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    padding-top: 16px;
    border-top: 1px solid $border-color;
  }

  &__tag {
    font-size: 0.72rem;
    font-weight: 500;
    font-family: $font-mono;
    padding: 4px 10px;
    border-radius: 100px;
    border: 1px solid;
  }
}
</style>
