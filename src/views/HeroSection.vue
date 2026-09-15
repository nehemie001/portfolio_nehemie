<script setup>
import { ArrowDown, ArrowUpRight, Mail, MapPin } from 'lucide-vue-next'

const featured = [
  { index: '01', title: 'PRESTIGE-SMS', meta: 'SaaS · Angular', href: '#projects' },
  { index: '02', title: 'PRIMUS KARE', meta: 'Santé · Angular', href: '#projects' },
  { index: '03', title: 'Dépannage SONAP', meta: 'Mobile · Flutter', href: '#projects' },
]

function scrollTo(id) {
  const el = document.getElementById(id)
  if (el) window.scrollTo({ top: el.getBoundingClientRect().top + window.scrollY - 80, behavior: 'smooth' })
}
</script>

<template>
  <section id="hero" class="hero">
    <div class="hero__bg" aria-hidden="true">
      <div class="hero__wash"></div>
      <div class="hero__ring"></div>
    </div>

    <div class="container hero__layout">
      <div class="hero__main">
        <div class="hero__kicker">
          <span class="pulse-dot"></span>
          <MapPin :size="13" />
          Abidjan, Côte d'Ivoire
        </div>

        <p class="hero__hello">Bonjour, je suis</p>
        <h1 class="hero__name">
          <span class="hero__firstname">Néhémie</span>
          <span class="hero__lastname">Pédahel Kouyo</span>
        </h1>

        <p class="hero__role">
          Développeur <em>Front-end Angular</em> &amp; applications mobiles Flutter.
          J’aide les équipes produit à livrer des interfaces claires, rapides et maintenables.
        </p>

        <div class="hero__actions">
          <button class="btn-primary" @click="scrollTo('projects')">
            Voir les projets <ArrowUpRight :size="16" />
          </button>
          <a href="mailto:kouyonehemiepedahel@gmail.com" class="btn-outline">
            <Mail :size="16" /> Me écrire
          </a>
        </div>

        <dl class="hero__facts">
          <div>
            <dt>Expérience</dt>
            <dd>4+ ans</dd>
          </div>
          <div>
            <dt>Focus</dt>
            <dd>Angular · Flutter</dd>
          </div>
          <div>
            <dt>Actuellement</dt>
            <dd>Kyrmann SE</dd>
          </div>
        </dl>
      </div>

      <aside class="hero__aside">
        <p class="hero__aside-label">Travaux récents</p>
        <div class="hero__works">
          <a
            v-for="item in featured"
            :key="item.index"
            :href="item.href"
            class="hero__work"
            @click.prevent="scrollTo('projects')"
          >
            <span class="hero__work-index">{{ item.index }}</span>
            <span class="hero__work-body">
              <strong>{{ item.title }}</strong>
              <small>{{ item.meta }}</small>
            </span>
            <ArrowUpRight :size="16" class="hero__work-icon" />
          </a>
        </div>
      </aside>
    </div>

    <button class="hero__scroll" @click="scrollTo('about')" aria-label="Découvrir le profil">
      <ArrowDown :size="18" />
    </button>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  overflow: hidden;
  padding: 140px 0 96px;

  &__bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  &__wash {
    position: absolute;
    width: 70vw;
    height: 70vw;
    max-width: 780px;
    max-height: 780px;
    right: -12%;
    top: -10%;
    background: radial-gradient(circle, rgba($accent, 0.22) 0%, transparent 68%);
  }

  &__ring {
    position: absolute;
    width: 520px;
    height: 520px;
    right: 6%;
    top: 18%;
    border: 1px solid rgba($accent, 0.12);
    display: none;
  }

  &__layout {
    position: relative;
    z-index: 1;
    display: flex;
    flex-direction: column;
    gap: 56px;
  }

  &__kicker {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 6px 14px;
    border: 1px solid $border-color;
    border-radius: 999px;
    font-size: 0.78rem;
    color: $text-secondary;
    margin-bottom: 28px;
    background: rgba($bg-card, 0.6);
  }

  .pulse-dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: #7dba6a;
    box-shadow: 0 0 0 0 rgba(125, 186, 106, 0.5);
    animation: pulse 2s infinite;
  }

  &__hello {
    font-family: $font-mono;
    font-size: 0.82rem;
    letter-spacing: 0.16em;
    text-transform: uppercase;
    color: $accent;
    margin-bottom: 12px;
  }

  &__name {
    font-size: clamp(2.6rem, 6vw, 4.8rem);
    font-weight: 800;
    line-height: 0.96;
    margin-bottom: 24px;
    letter-spacing: -0.03em;
  }

  &__firstname,
  &__lastname {
    display: block;
  }

  &__lastname {
    color: $accent;
  }

  &__role {
    font-size: clamp(1.02rem, 2vw, 1.18rem);
    color: $text-secondary;
    max-width: 540px;
    line-height: 1.75;
    margin-bottom: 36px;

    em {
      font-style: normal;
      color: $text-primary;
      font-weight: 600;
    }
  }

  &__actions {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 48px;
  }

  &__facts {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 20px;
    padding-top: 28px;
    border-top: 1px solid $border-color;
    max-width: 560px;

    @media (max-width: 540px) {
      grid-template-columns: 1fr;
      gap: 16px;
    }

    dt {
      font-family: $font-mono;
      font-size: 0.68rem;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      color: $text-muted;
      margin-bottom: 4px;
    }

    dd {
      font-family: $font-heading;
      font-size: 1.02rem;
      font-weight: 700;
    }
  }

  &__aside {
    width: 100%;
    max-width: 100%;
  }

  &__aside-label {
    font-family: $font-mono;
    font-size: 0.68rem;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: $text-muted;
    margin-bottom: 14px;
  }

  &__works {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;

    @media (max-width: 720px) {
      grid-template-columns: 1fr;
    }
  }

  &__work {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 16px 18px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    transition: $transition-fast;

    &:hover {
      border-color: rgba($accent, 0.4);
      .hero__work-icon { color: $accent; transform: translate(2px, -2px); }
      strong { color: $accent; }
    }
  }

  &__work-index {
    font-family: $font-mono;
    font-size: 0.75rem;
    color: $accent;
    width: 28px;
  }

  &__work-body {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 2px;

    strong {
      font-family: $font-heading;
      font-size: 0.98rem;
      font-weight: 700;
      transition: $transition-fast;
    }

    small {
      color: $text-muted;
      font-size: 0.78rem;
    }
  }

  &__work-icon {
    color: $text-muted;
    transition: $transition-fast;
  }

  &__scroll {
    position: absolute;
    bottom: 32px;
    left: 50%;
    transform: translateX(-50%);
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 1px solid $border-color;
    color: $text-muted;
    display: grid;
    place-items: center;
    animation: bounce 2.4s ease-in-out infinite;
    z-index: 1;

    &:hover { color: $accent; border-color: $accent; }
  }
}

@keyframes pulse {
  0%   { box-shadow: 0 0 0 0 rgba(125, 186, 106, 0.5); }
  70%  { box-shadow: 0 0 0 8px rgba(125, 186, 106, 0); }
  100% { box-shadow: 0 0 0 0 rgba(125, 186, 106, 0); }
}

@keyframes bounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50%       { transform: translateX(-50%) translateY(7px); }
}
</style>
