<script setup>
import { ref, onMounted } from 'vue'
import { GraduationCap, Award, Calendar, ExternalLink } from 'lucide-vue-next'

const sectionRef = ref(null)

const education = [
  {
    id: 1,
    degree: 'Formation ADACI',
    school: 'Atos CI',
    period: '2024',
    type: 'Certification',
    color: '#22d3ee',
    desc: 'Formation en architecture et développement d\'applications d\'entreprise. Approche microservices, conteneurisation Docker, bonnes pratiques DevOps.',
    icon: Award,
  },
  {
    id: 2,
    degree: 'Certificat Spring Boot – Udemy',
    school: 'Udemy',
    period: '2024',
    type: 'Certificat en ligne',
    color: '#6366f1',
    desc: 'Développement d\'APIs REST robustes avec Spring Boot, Spring Security, JPA/Hibernate et déploiement sur serveur cloud.',
    icon: Award,
  },
  {
    id: 3,
    degree: 'Soft Skills & Leadership',
    school: 'Nabou Fall Akademy',
    period: '2024',
    type: 'Formation',
    color: '#ec4899',
    desc: 'Développement personnel et professionnel : communication interpersonnelle, gestion du stress, leadership situationnel et travail en équipe.',
    icon: GraduationCap,
  },
  {
    id: 4,
    degree: 'DevFest Cloud Abidjan',
    school: 'Google Developer Groups',
    period: '2024',
    type: 'Conférence',
    color: '#f59e0b',
    desc: 'Participation au DevFest d\'Abidjan – conférences sur le Cloud Computing, l\'IA générative, Firebase et les innovations Google.',
    icon: Award,
  },
  {
    id: 5,
    degree: 'BTS – Développement Informatique',
    school: 'Institut de Formation Sainte Marie',
    period: '2019 – 2020',
    type: 'Diplôme',
    color: '#8b5cf6',
    desc: 'Brevet de Technicien Supérieur en informatique de gestion, spécialité développement logiciel. Bases solides en algorithmique, programmation et bases de données.',
    icon: GraduationCap,
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .edu__card').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="education" class="section education" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">// Formation</span>
        <h2>Parcours académique</h2>
        <p>Diplômes, certifications et formations qui jalonnent mon évolution continue.</p>
      </div>

      <div class="edu__grid">
        <article
          v-for="(item, i) in education"
          :key="item.id"
          class="edu__card"
          :style="`--edu-color: ${item.color}; --delay: ${i * 0.1}s`"
        >
          <div class="edu__card-top">
            <div class="edu__icon" :style="`color: ${item.color}; background: ${item.color}15`">
              <component :is="item.icon" :size="22" />
            </div>
            <div class="edu__badge" :style="`color: ${item.color}; border-color: ${item.color}30; background: ${item.color}10`">
              {{ item.type }}
            </div>
          </div>

          <h3 class="edu__degree">{{ item.degree }}</h3>
          <p class="edu__school">{{ item.school }}</p>

          <div class="edu__period">
            <Calendar :size="13" />
            {{ item.period }}
          </div>

          <p class="edu__desc">{{ item.desc }}</p>

          <div class="edu__border-glow" :style="`background: ${item.color}`"></div>
        </article>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.education {
  background: $bg-secondary;
}

.edu {
  &__grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;

    @media (max-width: 640px) {
      grid-template-columns: 1fr;
    }
  }

  &__card {
    position: relative;
    padding: 26px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    display: flex;
    flex-direction: column;
    gap: 12px;
    overflow: hidden;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s ease, transform 0.6s ease, border-color 0.3s, box-shadow 0.3s;
    transition-delay: var(--delay);

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }

    &:hover {
      border-color: var(--edu-color, $accent-blue);
      background: $bg-card-hover;
      transform: translateY(-4px);
      box-shadow: 0 12px 40px rgba(0,0,0,0.3);

      .edu__border-glow { opacity: 0.6; }
    }
  }

  &__border-glow {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 3px;
    opacity: 0.3;
    transition: $transition-base;
  }

  &__card-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 12px;
  }

  &__icon {
    width: 46px; height: 46px;
    border-radius: $radius-sm;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  &__badge {
    font-size: 0.7rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    padding: 4px 10px;
    border-radius: 100px;
    border: 1px solid;
    font-family: $font-mono;
    white-space: nowrap;
  }

  &__degree {
    font-size: 1rem;
    font-weight: 700;
    color: $text-primary;
    line-height: 1.4;
  }

  &__school {
    font-size: 0.88rem;
    font-weight: 600;
    color: var(--edu-color, $accent-blue);
  }

  &__period {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.8rem;
    color: $text-muted;
    font-family: $font-mono;

    svg { flex-shrink: 0; }
  }

  &__desc {
    font-size: 0.85rem;
    color: $text-secondary;
    line-height: 1.75;
  }
}
</style>
