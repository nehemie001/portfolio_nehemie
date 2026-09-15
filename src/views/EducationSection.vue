<script setup>
import { onMounted, ref } from 'vue'
import { GraduationCap, Award, Calendar } from 'lucide-vue-next'

const sectionRef = ref(null)

const education = [
  {
    id: 1,
    degree: 'Formation ADACI',
    school: 'Atos CI',
    period: '2024',
    type: 'Certification',
    desc: 'Architecture d’applications d’entreprise, microservices, Docker, pratiques DevOps.',
    icon: Award,
  },
  {
    id: 2,
    degree: 'Spring Boot — Udemy',
    school: 'Udemy',
    period: '2024',
    type: 'Certificat',
    desc: 'APIs REST, Spring Security, JPA/Hibernate et déploiement cloud.',
    icon: Award,
  },
  {
    id: 3,
    degree: 'Soft Skills & Leadership',
    school: 'Nabou Fall Akademy',
    period: '2024',
    type: 'Formation',
    desc: 'Communication, gestion du stress, leadership situationnel, travail en équipe.',
    icon: GraduationCap,
  },
  {
    id: 4,
    degree: 'DevFest Cloud Abidjan',
    school: 'Google Developer Groups',
    period: '2024',
    type: 'Conférence',
    desc: 'Cloud, IA générative, Firebase et écosystème Google.',
    icon: Award,
  },
  {
    id: 5,
    degree: 'BTS — Développement Informatique',
    school: 'Institut de Formation Sainte Marie',
    period: '2019 – 2020',
    type: 'Diplôme',
    desc: 'Informatique de gestion : algorithmique, programmation, bases de données.',
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
        <span class="section-label">05 — Formation</span>
        <h2>Parcours académique</h2>
        <p>Diplômes, certifications et formations qui jalonnent une pratique en continu.</p>
      </div>

      <div class="edu">
        <article
          v-for="(item, i) in education"
          :key="item.id"
          class="edu__card"
          :style="`--delay: ${i * 0.08}s`"
        >
          <div class="edu__top">
            <div class="edu__icon">
              <component :is="item.icon" :size="18" />
            </div>
            <span>{{ item.type }}</span>
          </div>
          <h3>{{ item.degree }}</h3>
          <p class="edu__school">{{ item.school }}</p>
          <p class="edu__period"><Calendar :size="13" /> {{ item.period }}</p>
          <p class="edu__desc">{{ item.desc }}</p>
        </article>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.education {
  background: $bg-secondary;
  border-top: 1px solid $border-color;
}

.edu {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;

  @media (max-width: 900px) {
    grid-template-columns: 1fr;
  }

  &__card {
    padding: 24px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    display: flex;
    flex-direction: column;
    gap: 8px;
    opacity: 0;
    transform: translateY(16px);
    transition: $transition-base;
    transition-delay: var(--delay);

    &.visible {
      opacity: 1;
      transform: none;
    }

    &:hover {
      border-color: rgba($accent, 0.4);
      transform: translateY(-3px);
    }
  }

  &__top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 6px;

    span {
      font-family: $font-mono;
      font-size: 0.68rem;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: $accent;
      border: 1px solid rgba($accent, 0.25);
      padding: 3px 8px;
      border-radius: 100px;
    }
  }

  &__icon {
    width: 36px;
    height: 36px;
    border-radius: 8px;
    display: grid;
    place-items: center;
    color: $accent;
    background: rgba($accent, 0.1);
  }

  h3 {
    font-size: 1rem;
    line-height: 1.35;
  }

  &__school {
    font-weight: 600;
    color: $accent-soft;
    font-size: 0.88rem;
  }

  &__period {
    display: flex;
    align-items: center;
    gap: 6px;
    font-size: 0.78rem;
    color: $text-muted;
    font-family: $font-mono;
  }

  &__desc {
    font-size: 0.85rem;
    color: $text-secondary;
    line-height: 1.7;
  }
}
</style>
