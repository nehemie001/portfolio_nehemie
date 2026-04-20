<script setup>
import { ref, onMounted, nextTick } from 'vue'
import {
  Globe, Smartphone, Server, Database,
  GitBranch, Package, Cloud, Cpu,
  Users, Clock, Star, MessageSquare
} from 'lucide-vue-next'

const sectionRef = ref(null)

const techSkills = [
  { name: 'Angular',            level: 55, icon: Globe,    color: '#dd0031', category: 'Front-end' },
  { name: 'Vue.js',             level: 75, icon: Globe,    color: '#42b883', category: 'Front-end' },
  { name: 'TypeScript',         level: 65, icon: Cpu,      color: '#3178c6', category: 'Front-end' },
  { name: 'HTML / CSS / SCSS',  level: 85, icon: Globe,    color: '#e34f26', category: 'Front-end' },
  { name: 'Flutter / Dart',     level: 75, icon: Smartphone,color: '#54c5f8', category: 'Mobile' },
  { name: 'Laravel',            level: 65, icon: Server,   color: '#ff2d20', category: 'Back-end' },
  { name: 'Spring Boot',        level: 65, icon: Server,   color: '#6db33f', category: 'Back-end' },
  { name: 'PostgreSQL',         level: 70, icon: Database, color: '#336791', category: 'Base de données' },
  { name: 'Docker',             level: 72, icon: Package,  color: '#2496ed', category: 'DevOps' },
  { name: 'WordPress',          level: 80, icon: Globe,    color: '#21759b', category: 'CMS' },
  { name: 'Pinia / RxJS',       level: 74, icon: GitBranch,color: '#8b5cf6', category: 'État' },
  { name: 'API REST / Micro.',  level: 73, icon: Cloud,    color: '#6366f1', category: 'Architecture' },
]

const softSkills = [
  { name: 'Travail en équipe', icon: Users,         color: '#6366f1' },
  { name: 'Gestion du temps',  icon: Clock,         color: '#8b5cf6' },
  { name: 'Leadership',        icon: Star,          color: '#ec4899' },
  { name: 'Communication',     icon: MessageSquare, color: '#22d3ee' },
  { name: 'Pensée critique',   icon: Cpu,           color: '#f59e0b' },
]

const activeFilter = ref('Tous')
const filters = ['Tous', 'Front-end', 'Mobile', 'Back-end', 'DevOps', 'Architecture']

const filteredSkills = ref(techSkills)

function setFilter(f) {
  activeFilter.value = f
  filteredSkills.value = f === 'Tous' ? techSkills : techSkills.filter(s => s.category === f)
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) {
        e.target.classList.add('visible')
        // Animate bars
        e.target.querySelectorAll('.skills__bar-fill').forEach(bar => {
          bar.style.width = bar.dataset.level + '%'
        })
      }
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .reveal-left, .reveal-right, .skills__card').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="skills" class="section skills" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">// Compétences</span>
        <h2>Mon arsenal technique</h2>
        <p>Des technologies soigneusement maîtrisées pour des solutions complètes, du front-end au déploiement.</p>
      </div>

      <!-- Filter buttons -->
      <div class="skills__filters reveal">
        <button
          v-for="f in filters"
          :key="f"
          class="skills__filter-btn"
          :class="{ active: activeFilter === f }"
          @click="setFilter(f)"
        >{{ f }}</button>
      </div>

      <!-- Tech skill cards -->
      <div class="skills__grid">
        <div
          v-for="(skill, i) in filteredSkills"
          :key="skill.name"
          class="skills__card reveal"
          :style="`--delay: ${i * 0.06}s; --skill-color: ${skill.color}`"
        >
          <div class="skills__card-header">
            <div class="skills__icon" :style="`color: ${skill.color}; background: ${skill.color}18`">
              <component :is="skill.icon" :size="20" />
            </div>
            <div class="skills__card-info">
              <span class="skills__name">{{ skill.name }}</span>
              <span class="skills__category">{{ skill.category }}</span>
            </div>
            <span class="skills__percent">{{ skill.level }}%</span>
          </div>
          <div class="skills__bar">
            <div
              class="skills__bar-fill"
              :data-level="skill.level"
              :style="`background: linear-gradient(90deg, ${skill.color}, ${skill.color}99)`"
            ></div>
          </div>
        </div>
      </div>

      <!-- Soft skills -->
      <div class="skills__soft reveal">
        <h3 class="skills__soft-title">Soft Skills</h3>
        <div class="skills__soft-grid">
          <div
            v-for="s in softSkills"
            :key="s.name"
            class="skills__soft-card"
            :style="`--c: ${s.color}`"
          >
            <div class="skills__soft-icon">
              <component :is="s.icon" :size="22" />
            </div>
            <span>{{ s.name }}</span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.skills {
  background: $bg-primary;

  // ── Filters ────────────────────────────────────────────
  &__filters {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    margin-bottom: 48px;
  }

  &__filter-btn {
    padding: 8px 20px;
    border-radius: 100px;
    font-size: 0.82rem;
    font-weight: 500;
    color: $text-secondary;
    background: rgba(255,255,255,0.04);
    border: 1px solid $border-color;
    transition: $transition-fast;
    cursor: pointer;

    &:hover {
      border-color: $accent-blue;
      color: $accent-blue;
    }

    &.active {
      background: $gradient-main;
      border-color: transparent;
      color: #fff;
    }
  }

  // ── Cards grid ───────────────────────────────────────────
  &__grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 16px;
    margin-bottom: 64px;

    @media (max-width: 640px) {
      grid-template-columns: 1fr;
    }
  }

  &__card {
    padding: 20px 22px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    transition: $transition-base;
    transition-delay: var(--delay);
    opacity: 0;
    transform: translateY(20px);

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }

    &:hover {
      border-color: var(--skill-color, $accent-blue);
      background: $bg-card-hover;
      box-shadow: 0 4px 24px rgba(0,0,0,0.3), 0 0 0 1px var(--skill-color) inset;
      transform: translateY(-2px);
    }
  }

  &__card-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 14px;
  }

  &__icon {
    width: 40px; height: 40px;
    border-radius: $radius-sm;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  &__card-info {
    display: flex;
    flex-direction: column;
    gap: 2px;
    flex: 1;
    min-width: 0;
  }

  &__name {
    font-size: 0.92rem;
    font-weight: 600;
    color: $text-primary;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  &__category {
    font-size: 0.72rem;
    color: $text-muted;
    font-family: $font-mono;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  &__percent {
    font-size: 0.85rem;
    font-weight: 700;
    color: var(--skill-color, $accent-blue);
    font-family: $font-mono;
    flex-shrink: 0;
  }

  &__bar {
    height: 6px;
    background: rgba(255,255,255,0.07);
    border-radius: 3px;
    overflow: hidden;
  }

  &__bar-fill {
    height: 100%;
    width: 0;
    border-radius: 3px;
    transition: width 1.2s cubic-bezier(0.4, 0, 0.2, 1) 0.4s;
  }

  // ── Soft skills ──────────────────────────────────────────
  &__soft {
    &-title {
      font-size: 1.2rem;
      font-weight: 700;
      color: $text-primary;
      margin-bottom: 24px;
      text-align: center;

      &::after {
        content: '';
        display: block;
        width: 40px;
        height: 3px;
        background: $gradient-main;
        border-radius: 2px;
        margin: 8px auto 0;
      }
    }

    &-grid {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      justify-content: center;
    }

    &-card {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 14px 22px;
      background: $bg-card;
      border: 1px solid $border-color;
      border-radius: $radius-md;
      font-size: 0.9rem;
      font-weight: 500;
      color: $text-secondary;
      transition: $transition-base;
      cursor: default;

      &:hover {
        border-color: var(--c, $accent-blue);
        color: $text-primary;
        background: $bg-card-hover;
        transform: translateY(-3px);
        box-shadow: 0 8px 24px rgba(0,0,0,0.25);
      }
    }

    &-icon {
      color: var(--c, $accent-blue);
    }
  }
}
</style>
