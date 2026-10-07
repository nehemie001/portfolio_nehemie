<script setup>
import { ref, onMounted } from 'vue'

const sectionRef = ref(null)

const groups = [
  {
    title: 'Front-end',
    items: ['Angular', 'Vue.js', 'TypeScript', 'JavaScript', 'RxJS', 'HTML / CSS / SCSS', 'Pinia'],
  },
  {
    title: 'Mobile',
    items: ['Flutter', 'Dart', 'Bloc / Provider', 'iOS', 'Android'],
  },
  {
    title: 'Back-end & data',
    items: ['Spring Boot', 'Laravel', 'Java', 'PHP', 'PostgreSQL', 'MySQL', 'API REST', 'Swagger'],
  },
  {
    title: 'Outils & delivery',
    items: ['Git', 'Docker', 'Figma', 'Jira', 'Scrum', 'WordPress'],
  },
]

const softSkills = ['Travail en équipe', 'Gestion du temps', 'Leadership', 'Communication', 'Esprit critique']

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="skills" class="section skills" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">02 — Compétences</span>
        <h2>Front, mobile, back — le même fil.</h2>
        <p>Les briques que j’assemble au quotidien pour concevoir, livrer et faire vivre un produit de bout en bout.</p>
      </div>

      <div class="skills__bento">
        <article
          v-for="(group, i) in groups"
          :key="group.title"
          class="skills__group reveal"
          :style="`--delay: ${i * 0.08}s`"
        >
          <h3>{{ group.title }}</h3>
          <ul>
            <li v-for="item in group.items" :key="item">{{ item }}</li>
          </ul>
        </article>
      </div>

      <div class="skills__soft reveal">
        <h3>Aussi</h3>
        <div class="skills__chips">
          <span v-for="s in softSkills" :key="s">{{ s }}</span>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.skills {
  background: $bg-primary;

  &__bento {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
    margin-bottom: 40px;

    @media (max-width: 720px) {
      grid-template-columns: 1fr;
    }
  }

  &__group {
    padding: 28px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    border-top: 2px solid $accent;
    transition: $transition-base;
    transition-delay: var(--delay);
    opacity: 0;
    transform: translateY(16px);

    &:nth-child(2) { border-top-color: $accent-blue; }
    &:nth-child(3) { border-top-color: $accent-soft; }
    &:nth-child(4) { border-top-color: $accent-violet; }

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }

    &:hover {
      border-color: rgba($accent, 0.35);
    }

    h3 {
      font-size: 1.05rem;
      margin-bottom: 18px;
      color: $accent-soft;
    }

    ul {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    li {
      font-size: 0.84rem;
      font-weight: 500;
      padding: 7px 12px;
      border-radius: 100px;
      background: rgba($text-primary, 0.04);
      border: 1px solid $border-color;
      color: $text-secondary;
    }
  }

  &__soft {
    display: flex;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
    padding-top: 8px;

    h3 {
      font-family: $font-mono;
      font-size: 0.72rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: $text-muted;
      font-weight: 500;
    }
  }

  &__chips {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;

    span {
      font-size: 0.84rem;
      padding: 8px 14px;
      border-radius: 100px;
      border: 1px dashed rgba($accent, 0.35);
      color: $text-secondary;
    }
  }
}
</style>
