<script setup>
import { computed, nextTick, onMounted, ref, watch } from 'vue'
import { ArrowUpRight, MessageSquare, HeartPulse, Smartphone, Globe } from 'lucide-vue-next'

const sectionRef = ref(null)
const activeFilter = ref('Tous')
const filters = ['Tous', 'Web', 'Mobile']

const projects = [
  {
    id: 1,
    featured: true,
    title: 'PRESTIGE-SMS',
    subtitle: 'Plateforme de marketing digital par SMS',
    icon: MessageSquare,
    description:
      'Interface web pour créer, programmer et suivre des campagnes SMS. Gestion des répertoires, envois groupés, tarification dégressive et tableau de bord des campagnes — de l’inscription à l’envoi en un clic.',
    tags: ['Angular', 'TypeScript', 'RxJS', 'API REST', 'SCSS'],
    category: 'Web',
    year: '2025',
    role: 'Front-end Angular',
    features: ['Campagnes SMS', 'Envois programmés', 'Répertoire de contacts', 'Suivi des envois'],
    href: 'https://prestige-sms.com/',
    mock: 'sms',
  },
  {
    id: 2,
    featured: true,
    title: 'PRIMUS KARE',
    subtitle: 'Plateforme web de parcours de soins',
    icon: HeartPulse,
    description:
      'Application métier santé : suivi des patients, workflows de validation des actes, tableaux de bord et reporting pour les équipes soignantes et administratives. Conçue pour des parcours clairs, traçables et utilisables au quotidien.',
    tags: ['Angular', 'TypeScript', 'RxJS', 'Angular Material', 'API REST'],
    category: 'Web',
    year: '2025 – 2026',
    role: 'Front-end Angular',
    features: ['Dossiers patients', 'Workflows de soins', 'Tableaux de bord', 'Reporting'],
    href: null,
    mock: 'kare',
  },
  {
    id: 3,
    featured: false,
    title: 'Dépannage SONAP',
    subtitle: 'App mobile — côté client',
    icon: Smartphone,
    description:
      'Application Flutter pour conducteurs en panne : géolocalisation, mise en relation avec un garagiste proche et notifications push.',
    tags: ['Flutter', 'Dart', 'Firebase', 'Géolocalisation'],
    category: 'Mobile',
    year: '2025',
    role: 'Mobile Flutter',
    features: ['GPS temps réel', 'Mise en relation', 'Notifications', 'Suivi d’intervention'],
    href: null,
    mock: null,
  },
  {
    id: 4,
    featured: false,
    title: 'Dépannage SONAP Pro',
    subtitle: 'App mobile — côté garagiste',
    icon: Smartphone,
    description:
      'Pendant professionnel : demandes d’intervention, devis, crédits, paiement mobile et tableau de bord.',
    tags: ['Flutter', 'Dart', 'Paiement mobile', 'API REST'],
    category: 'Mobile',
    year: '2025',
    role: 'Mobile Flutter',
    features: ['Devis', 'Crédits', 'Paiement mobile', 'Dashboard'],
    href: null,
    mock: null,
  },
  {
    id: 5,
    featured: false,
    title: 'ODISSEY — OGS Prestataire',
    subtitle: 'Module web assurance santé',
    icon: Globe,
    description:
      'Espace prestataires de soins : gestion des patients assurés, validation des actes, dashboards et rapports.',
    tags: ['Angular', 'TypeScript', 'RxJS', 'API REST'],
    category: 'Web',
    year: '2025',
    role: 'Front-end Angular',
    features: ['Patients', 'Validation d’actes', 'Dashboards', 'Rapports'],
    href: null,
    mock: null,
  },
]

const filtered = computed(() =>
  activeFilter.value === 'Tous'
    ? projects
    : projects.filter(p => p.category === activeFilter.value)
)

const featured = computed(() => filtered.value.filter(p => p.featured))
const others = computed(() => filtered.value.filter(p => !p.featured))

function revealEls() {
  sectionRef.value?.querySelectorAll('.reveal, .project-card, .case').forEach(el => {
    el.classList.add('visible')
  })
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.08 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .project-card, .case').forEach(el => observer.observe(el))
})

watch(activeFilter, async () => {
  await nextTick()
  revealEls()
})
</script>

<template>
  <section id="projects" class="section projects" ref="sectionRef">
    <div class="container">
      <div class="projects__head">
        <div class="section-header reveal">
          <span class="section-label">04 — Projets</span>
          <h2>Produits livrés.</h2>
          <p>Du SaaS SMS à la santé, en passant par le dépannage automobile — des interfaces utilisées en conditions réelles.</p>
        </div>

        <div class="projects__filters reveal">
          <button
            v-for="f in filters"
            :key="f"
            class="projects__filter"
            :class="{ active: activeFilter === f }"
            @click="activeFilter = f"
          >{{ f }}</button>
        </div>
      </div>

      <div v-if="featured.length" class="projects__featured">
        <article
          v-for="(project, i) in featured"
          :key="project.id"
          class="case"
          :class="{ reverse: i % 2 === 1 }"
          :style="`--delay: ${i * 0.1}s`"
        >
          <div class="case__visual" :class="`case__visual--${project.mock}`">
            <div v-if="project.mock === 'sms'" class="mock mock--sms">
              <div class="mock__bar">
                <span></span><span></span><span></span>
                <p>PRESTIGE-SMS</p>
              </div>
              <div class="mock__stats">
                <div><small>Envoyés</small><b>12 480</b></div>
                <div><small>Délivrés</small><b>98,4%</b></div>
                <div><small>Campagnes</small><b>24</b></div>
              </div>
              <div class="mock__msg">
                <span>Campagne · Promo Q3</span>
                <p>Profitez de -20% ce week-end. Répondez STOP pour vous désinscrire.</p>
              </div>
              <div class="mock__list">
                <div v-for="n in 3" :key="n" class="mock__row">
                  <i></i>
                  <span>Envoi #{{ 1840 + n }}</span>
                  <em>OK</em>
                </div>
              </div>
            </div>

            <div v-else-if="project.mock === 'kare'" class="mock mock--kare">
              <div class="mock__bar">
                <span></span><span></span><span></span>
                <p>PRIMUS KARE</p>
              </div>
              <div class="mock__kare-grid">
                <div class="mock__patient">
                  <div class="avatar"></div>
                  <div>
                    <b>Dossier patient</b>
                    <small>Suivi · Actes · Historique</small>
                  </div>
                </div>
                <div class="mock__kpis">
                  <div><small>RDV du jour</small><b>18</b></div>
                  <div><small>En attente</small><b>05</b></div>
                </div>
                <div class="mock__table">
                  <div v-for="row in ['Consultation', 'Biologie', 'Ordonnance']" :key="row">
                    <span>{{ row }}</span>
                    <em>Validé</em>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div class="case__content">
            <span class="case__index">0{{ i + 1 }} — {{ project.category }}</span>
            <h3>{{ project.title }}</h3>
            <p class="case__sub">{{ project.subtitle }}</p>
            <p class="case__desc">{{ project.description }}</p>
            <ul class="case__feats">
              <li v-for="feat in project.features" :key="feat">{{ feat }}</li>
            </ul>
            <div class="case__tags">
              <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
            </div>
            <div class="case__meta">
              <span>{{ project.role }}</span>
              <span>{{ project.year }}</span>
            </div>
            <a
              v-if="project.href"
              :href="project.href"
              class="case__link"
              target="_blank"
              rel="noopener noreferrer"
            >
              Voir le site <ArrowUpRight :size="16" />
            </a>
          </div>
        </article>
      </div>

      <div v-if="others.length" class="projects__grid">
        <article
          v-for="(project, i) in others"
          :key="project.id"
          class="project-card"
          :style="`--delay: ${i * 0.08}s`"
        >
          <div class="project-card__icon">
            <component :is="project.icon" :size="22" />
          </div>
          <span class="project-card__cat">{{ project.category }}</span>
          <h3>{{ project.title }}</h3>
          <p class="project-card__sub">{{ project.subtitle }}</p>
          <p class="project-card__desc">{{ project.description }}</p>
          <ul>
            <li v-for="feat in project.features" :key="feat">{{ feat }}</li>
          </ul>
          <div class="project-card__tags">
            <span v-for="tag in project.tags" :key="tag">{{ tag }}</span>
          </div>
        </article>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.projects {
  background: $bg-primary;
  border-top: 1px solid $border-color;

  &__head {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    gap: 24px;
    margin-bottom: 56px;

    .section-header { margin-bottom: 0; }

    @media (max-width: 800px) {
      flex-direction: column;
      align-items: flex-start;
    }
  }

  &__filters {
    display: flex;
    gap: 8px;
    flex-shrink: 0;
  }

  &__filter {
    padding: 8px 18px;
    border-radius: 8px;
    font-size: 0.82rem;
    font-weight: 600;
    color: $text-secondary;
    border: 1px solid $border-color;
    background: transparent;
    transition: $transition-fast;

    &:hover { color: $text-primary; border-color: $accent; }

    &.active {
      background: $accent;
      border-color: $accent;
      color: $on-accent;
    }
  }

  &__featured {
    display: flex;
    flex-direction: column;
    gap: 80px;
    margin-bottom: 80px;

    @media (max-width: 800px) { gap: 56px; margin-bottom: 56px; }
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;

    @media (max-width: 980px) {
      grid-template-columns: 1fr;
    }
  }
}

.case {
  display: grid;
  grid-template-columns: 1.05fr 0.95fr;
  gap: 48px;
  align-items: center;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.7s ease, transform 0.7s ease;
  transition-delay: var(--delay);

  &.visible {
    opacity: 1;
    transform: none;
  }

  &.reverse {
    direction: rtl;
    > * { direction: ltr; }
  }

  @media (max-width: 900px) {
    grid-template-columns: 1fr;
    gap: 28px;
    &.reverse { direction: ltr; }
  }

  &__visual {
    min-height: 360px;
    border-radius: $radius-lg;
    border: 1px solid $border-color;
    background: $bg-card;
    padding: 22px;
    overflow: hidden;

    &--sms {
      background:
        radial-gradient(circle at 20% 10%, rgba($accent, 0.18), transparent 50%),
        $bg-card;
    }

    &--kare {
      background:
        radial-gradient(circle at 80% 0%, rgba($accent-teal, 0.2), transparent 48%),
        $bg-card;
    }
  }

  &__index {
    font-family: $font-mono;
    font-size: 0.72rem;
    letter-spacing: 0.16em;
    text-transform: uppercase;
    color: $accent;
    display: block;
    margin-bottom: 10px;
  }

  h3 {
    font-size: clamp(1.6rem, 3vw, 2.2rem);
    margin-bottom: 6px;
  }

  &__sub {
    color: $accent-soft;
    font-weight: 600;
    margin-bottom: 16px;
  }

  &__desc {
    color: $text-secondary;
    font-size: 0.95rem;
    line-height: 1.8;
    margin-bottom: 20px;
  }

  &__feats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px 16px;
    margin-bottom: 20px;

    li {
      font-size: 0.84rem;
      color: $text-secondary;
      padding-left: 12px;
      position: relative;

      &::before {
        content: '';
        position: absolute;
        left: 0;
        top: 0.55em;
        width: 5px;
        height: 5px;
        border-radius: 50%;
        background: $accent;
      }
    }
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-bottom: 16px;

    span {
      font-family: $font-mono;
      font-size: 0.7rem;
      padding: 4px 10px;
      border-radius: 6px;
      border: 1px solid $border-color;
      color: $text-muted;
    }
  }

  &__meta {
    display: flex;
    gap: 16px;
    font-size: 0.8rem;
    color: $text-muted;
    margin-bottom: 20px;
  }

  &__link {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-weight: 650;
    color: $ink;
    background: $accent;
    padding: 10px 16px;
    border-radius: 8px;
    font-size: 0.85rem;
    transition: $transition-fast;

    &:hover {
      background: $accent-soft;
    }
  }
}

.mock {
  height: 100%;
  min-height: 320px;
  background: #0a1016;
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 12px;
  padding: 14px;
  display: flex;
  flex-direction: column;
  gap: 12px;

  &__bar {
    display: flex;
    align-items: center;
    gap: 6px;
    padding-bottom: 10px;
    border-bottom: 1px solid rgba(255,255,255,0.06);

    span {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: #3a3530;

      &:nth-child(1) { background: #c45c3a; }
      &:nth-child(2) { background: #c9a227; }
      &:nth-child(3) { background: #5a9a4a; }
    }

    p {
      margin-left: 8px;
      font-family: $font-mono;
      font-size: 0.7rem;
      letter-spacing: 0.12em;
      color: $text-muted;
    }
  }

  &__stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;

    div {
      background: rgba($accent, 0.08);
      border: 1px solid rgba($accent, 0.14);
      border-radius: 8px;
      padding: 10px;
    }

    small {
      display: block;
      font-size: 0.65rem;
      color: $text-muted;
      margin-bottom: 4px;
    }

    b {
      font-family: $font-heading;
      font-size: 1.05rem;
      color: #93c5fd;
    }
  }

  &__msg {
    background: rgba(255,255,255,0.03);
    border-radius: 8px;
    padding: 12px;

    span {
      font-size: 0.68rem;
      color: #93c5fd;
      font-family: $font-mono;
    }

    p {
      font-size: 0.82rem;
      color: #cbd5e1;
      margin-top: 6px;
      line-height: 1.5;
    }
  }

  &__list { display: flex; flex-direction: column; gap: 8px; }

  &__row {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.78rem;
    color: #cbd5e1;

    i {
      width: 7px;
      height: 7px;
      border-radius: 50%;
      background: #5a9a4a;
    }

    em {
      margin-left: auto;
      font-style: normal;
      color: #7dba6a;
      font-size: 0.7rem;
    }
  }

  &__kare-grid {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  &__patient {
    display: flex;
    gap: 12px;
    align-items: center;
    padding: 12px;
    background: rgba($accent-teal, 0.08);
    border: 1px solid rgba($accent-teal, 0.18);
    border-radius: 10px;

    .avatar {
      width: 42px;
      height: 42px;
      border-radius: 50%;
      background: linear-gradient(135deg, $accent-teal, #2d6b62);
    }

    b { display: block; font-size: 0.9rem; }
    small { color: $text-muted; font-size: 0.75rem; }
  }

  &__kpis {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;

    div {
      padding: 12px;
      border-radius: 8px;
      background: rgba(255,255,255,0.03);
      border: 1px solid $border-color;
    }

    small { display: block; font-size: 0.65rem; color: $text-muted; }
    b { font-family: $font-heading; font-size: 1.3rem; color: $accent-teal; }
  }

  &__table {
    display: flex;
    flex-direction: column;
    gap: 6px;

    div {
      display: flex;
      justify-content: space-between;
      font-size: 0.8rem;
      padding: 8px 10px;
      border-radius: 6px;
      background: rgba(255,255,255,0.03);
      color: $text-secondary;
    }

    em {
      font-style: normal;
      color: $accent-teal;
      font-size: 0.72rem;
    }
  }
}

.project-card {
  padding: 24px;
  background: $bg-card;
  border: 1px solid $border-color;
  border-radius: $radius-lg;
  display: flex;
  flex-direction: column;
  gap: 10px;
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
    transform: translateY(-4px);
  }

  &__icon {
    width: 42px;
    height: 42px;
    border-radius: 10px;
    display: grid;
    place-items: center;
    color: $accent;
    background: rgba($accent, 0.1);
    margin-bottom: 6px;
  }

  &__cat {
    font-family: $font-mono;
    font-size: 0.68rem;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: $accent;
  }

  h3 { font-size: 1.1rem; }

  &__sub {
    font-size: 0.82rem;
    color: $accent-soft;
  }

  &__desc {
    font-size: 0.88rem;
    color: $text-secondary;
    line-height: 1.7;
    flex: 1;
  }

  ul {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;

    li {
      font-size: 0.75rem;
      color: $text-muted;
    }
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    padding-top: 10px;
    border-top: 1px solid $border-color;

    span {
      font-family: $font-mono;
      font-size: 0.68rem;
      color: $text-muted;
    }
  }
}
</style>
