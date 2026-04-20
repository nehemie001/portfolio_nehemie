<script setup>
import { ref, onMounted } from 'vue'
import { Smartphone, Globe, ExternalLink, Tag } from 'lucide-vue-next'

const sectionRef = ref(null)

const projects = [
  {
    id: 1,
    title: 'Dépannage SONAP',
    subtitle: 'App Mobile – Côté Client',
    icon: Smartphone,
    description:
      'Application mobile Flutter de services automobiles permettant aux conducteurs en panne de trouver rapidement un garagiste à proximité. Géolocalisation en temps réel, mise en relation clients-garagistes et notifications push instantanées.',
    tags: ['Flutter', 'Dart', 'API REST', 'Bloc/Provider', 'Géolocalisation', 'Firebase', 'Git'],
    color: '#54c5f8',
    gradient: 'linear-gradient(135deg, #54c5f8, #007aff)',
    category: 'Mobile',
    features: ['Géolocalisation GPS temps réel', 'Mise en relation instantanée', 'Notifications push', 'Suivi d\'intervention'],
  },
  {
    id: 2,
    title: 'Dépannage SONAP Pro',
    subtitle: 'App Mobile – Côté Garagiste',
    icon: Smartphone,
    description:
      'Pendant professionnel de SONAP destiné aux garagistes partenaires. Gestion des demandes d\'intervention, établissement de devis en ligne, système de crédits, paiement mobile intégré et tableau de bord analytique.',
    tags: ['Flutter', 'Dart', 'API REST', 'Bloc/Provider', 'Paiement Mobile', 'Git'],
    color: '#8b5cf6',
    gradient: 'linear-gradient(135deg, #8b5cf6, #6366f1)',
    category: 'Mobile',
    features: ['Gestion des devis', 'Système de crédits', 'Paiement mobile', 'Tableau de bord'],
  },
  {
    id: 3,
    title: 'ODISSEY – OGS Prestataire',
    subtitle: 'Application Web Angular',
    icon: Globe,
    description:
      'Module web d\'assurance santé dédié aux prestataires de soins. Interface de gestion des patients assurés, workflows de validation des actes médicaux, tableaux de bord analytiques et rapports détaillés.',
    tags: ['Angular', 'TypeScript', 'HTML/CSS', 'API REST', 'RxJS', 'Git'],
    color: '#6366f1',
    gradient: 'linear-gradient(135deg, #6366f1, #ec4899)',
    category: 'Web',
    features: ['Gestion des patients', 'Workflows de validation', 'Tableaux de bord', 'Rapports PDF'],
  },
]

const activeFilter = ref('Tous')
const filters = ['Tous', 'Mobile', 'Web']

const filteredProjects = ref(projects)

function setFilter(f) {
  activeFilter.value = f
  filteredProjects.value = f === 'Tous' ? projects : projects.filter(p => p.category === f)
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .projects__card').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="projects" class="section projects" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">// Projets</span>
        <h2>Réalisations notables</h2>
        <p>Quelques projets qui illustrent mes capacités à concevoir et livrer des solutions complètes.</p>
      </div>

      <!-- Filters -->
      <div class="projects__filters reveal">
        <button
          v-for="f in filters"
          :key="f"
          class="projects__filter-btn"
          :class="{ active: activeFilter === f }"
          @click="setFilter(f)"
        >{{ f }}</button>
      </div>

      <!-- Cards -->
      <div class="projects__grid">
        <article
          v-for="(project, i) in filteredProjects"
          :key="project.id"
          class="projects__card"
          :style="`--p-color: ${project.color}; --p-gradient: ${project.gradient}; --delay: ${i * 0.12}s`"
        >
          <!-- Card header -->
          <div class="projects__card-header">
            <div class="projects__icon-wrap" :style="`background: ${project.gradient}`">
              <component :is="project.icon" :size="28" color="#fff" />
            </div>
            <div class="projects__header-text">
              <span class="projects__category">{{ project.category }}</span>
              <h3 class="projects__title">{{ project.title }}</h3>
              <p class="projects__subtitle">{{ project.subtitle }}</p>
            </div>
          </div>

          <!-- Description -->
          <p class="projects__desc">{{ project.description }}</p>

          <!-- Features -->
          <ul class="projects__features">
            <li v-for="feat in project.features" :key="feat">
              <span class="projects__feat-dot"></span>
              {{ feat }}
            </li>
          </ul>

          <!-- Divider -->
          <div class="projects__divider"></div>

          <!-- Tags -->
          <div class="projects__tags">
            <span v-for="tag in project.tags" :key="tag" class="projects__tag">
              {{ tag }}
            </span>
          </div>

          <!-- Actions -->
          <div class="projects__actions">
            <a href="#" class="projects__action-btn">
              <ExternalLink :size="15" /> Voir le projet
            </a>
          </div>

          <!-- Glow effect -->
          <div class="projects__glow"></div>
        </article>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.projects {
  background: $bg-primary;

  &__filters {
    display: flex;
    gap: 8px;
    justify-content: center;
    margin-bottom: 48px;
  }

  &__filter-btn {
    padding: 8px 24px;
    border-radius: 100px;
    font-size: 0.85rem;
    font-weight: 500;
    color: $text-secondary;
    background: rgba(255,255,255,0.04);
    border: 1px solid $border-color;
    transition: $transition-fast;
    cursor: pointer;

    &:hover { border-color: $accent-blue; color: $accent-blue; }

    &.active {
      background: $gradient-main;
      border-color: transparent;
      color: #fff;
    }
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
    gap: 24px;

    @media (max-width: 640px) {
      grid-template-columns: 1fr;
    }
  }

  &__card {
    position: relative;
    display: flex;
    flex-direction: column;
    gap: 18px;
    padding: 28px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-xl;
    transition: $transition-base;
    transition-delay: var(--delay);
    opacity: 0;
    transform: translateY(24px);
    overflow: hidden;

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }

    &:hover {
      border-color: var(--p-color, $accent-blue);
      background: $bg-card-hover;
      transform: translateY(-6px);
      box-shadow: 0 20px 60px rgba(0,0,0,0.4), 0 0 0 1px var(--p-color) inset;

      .projects__glow { opacity: 1; }
    }
  }

  &__glow {
    position: absolute;
    top: -80px; right: -80px;
    width: 200px; height: 200px;
    border-radius: 50%;
    background: var(--p-color, $accent-blue);
    filter: blur(60px);
    opacity: 0;
    transition: $transition-slow;
    pointer-events: none;
  }

  &__card-header {
    display: flex;
    align-items: flex-start;
    gap: 16px;
  }

  &__icon-wrap {
    flex-shrink: 0;
    width: 60px; height: 60px;
    border-radius: $radius-md;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 8px 24px rgba(0,0,0,0.3);
  }

  &__header-text {
    display: flex;
    flex-direction: column;
    gap: 2px;
    min-width: 0;
  }

  &__category {
    font-size: 0.7rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--p-color, $accent-blue);
    font-family: $font-mono;
  }

  &__title {
    font-size: 1.12rem;
    font-weight: 700;
    color: $text-primary;
  }

  &__subtitle {
    font-size: 0.82rem;
    color: $text-muted;
    font-style: italic;
  }

  &__desc {
    font-size: 0.9rem;
    color: $text-secondary;
    line-height: 1.8;
    flex: 1;
  }

  &__features {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;

    li {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 0.8rem;
      color: $text-secondary;
    }
  }

  &__feat-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    background: var(--p-gradient, $gradient-main);
    flex-shrink: 0;
  }

  &__divider {
    height: 1px;
    background: $border-color;
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }

  &__tag {
    font-size: 0.72rem;
    font-weight: 500;
    font-family: $font-mono;
    padding: 4px 10px;
    border-radius: 100px;
    background: rgba($accent-blue, 0.08);
    border: 1px solid rgba($accent-blue, 0.15);
    color: $accent-blue;
  }

  &__actions {
    display: flex;
    gap: 12px;
  }

  &__action-btn {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-size: 0.83rem;
    font-weight: 600;
    color: var(--p-color, $accent-blue);
    padding: 8px 16px;
    border-radius: $radius-sm;
    border: 1px solid var(--p-color, $accent-blue);
    transition: $transition-fast;

    &:hover {
      background: var(--p-color, $accent-blue);
      color: #fff;
    }
  }
}
</style>
