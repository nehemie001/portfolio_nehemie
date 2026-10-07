<script setup>
import { ArrowDown, ArrowUpRight, Mail, MapPin } from 'lucide-vue-next'

const featured = [
  { index: '01', title: 'PRESTIGE-SMS', meta: 'SaaS SMS · Web', href: '#projects' },
  { index: '02', title: 'PRIMUS KARE', meta: 'Santé · Web', href: '#projects' },
  { index: '03', title: 'Dépannage SONAP', meta: 'Mobile · Flutter', href: '#projects' },
]

const layers = [
  { label: 'Front-end', items: ['Angular', 'Vue.js', 'TypeScript', 'RxJS'] },
  { label: 'Mobile', items: ['Flutter', 'Dart', 'iOS', 'Android'] },
  { label: 'Back-end', items: ['Spring Boot', 'Laravel', 'PostgreSQL', 'Docker'] },
]

const marquee = [
  'Angular', 'Vue.js', 'TypeScript', 'Flutter', 'Dart',
  'Spring Boot', 'Laravel', 'PHP', 'Java', 'PostgreSQL',
  'MySQL', 'Docker', 'RxJS', 'Pinia', 'WordPress',
]

function scrollTo(id) {
  const el = document.getElementById(id)
  if (el) window.scrollTo({ top: el.getBoundingClientRect().top + window.scrollY - 80, behavior: 'smooth' })
}
</script>

<template>
  <section id="hero" class="hero">
    <div class="hero__bg" aria-hidden="true">
      <div class="hero__wash hero__wash--amber"></div>
      <div class="hero__wash hero__wash--ice"></div>
      <div class="hero__grid"></div>
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
          Développeur <em>Full-Stack</em> — web, mobile et back-end.
          J’interviens de l’interface à l’API : Angular, Vue.js, Flutter,
          Spring Boot, Laravel, PostgreSQL et Docker.
        </p>

        <div class="hero__chips">
          <span>Angular</span>
          <span>Vue.js</span>
          <span>Flutter</span>
          <span>Spring Boot</span>
          <span>Laravel</span>
          <span>Docker</span>
        </div>

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
            <dt>Profil</dt>
            <dd>Full-Stack</dd>
          </div>
          <div>
            <dt>Expérience</dt>
            <dd>4+ ans</dd>
          </div>
          <div>
            <dt>Actuellement</dt>
            <dd>Kyrmann SE</dd>
          </div>
        </dl>
      </div>

      <aside class="hero__aside">
        <div class="hero__board">
          <p class="hero__board-label">Stack</p>
          <div class="hero__layers">
            <div v-for="layer in layers" :key="layer.label" class="hero__layer">
              <h3>{{ layer.label }}</h3>
              <ul>
                <li v-for="item in layer.items" :key="item">{{ item }}</li>
              </ul>
            </div>
          </div>
        </div>

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

    <div class="hero__marquee" aria-hidden="true">
      <div class="hero__track">
        <span v-for="(tech, i) in [...marquee, ...marquee]" :key="i">{{ tech }}</span>
      </div>
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
  flex-direction: column;
  justify-content: center;
  overflow: hidden;
  padding: 128px 0 0;

  &__bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  &__wash {
    position: absolute;
    border-radius: 50%;
    filter: blur(8px);

    &--amber {
      width: 55vw;
      height: 55vw;
      max-width: 640px;
      max-height: 640px;
      right: -8%;
      top: -12%;
      background: radial-gradient(circle, rgba($accent, 0.2) 0%, transparent 68%);
    }

    &--ice {
      width: 42vw;
      height: 42vw;
      max-width: 480px;
      max-height: 480px;
      left: -10%;
      bottom: 8%;
      background: radial-gradient(circle, rgba($accent-blue, 0.16) 0%, transparent 70%);
    }
  }

  &__grid {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba($accent-blue, 0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba($accent-blue, 0.04) 1px, transparent 1px);
    background-size: 56px 56px;
    mask-image: radial-gradient(ellipse 70% 60% at 50% 30%, black, transparent);
  }

  &__layout {
    position: relative;
    z-index: 1;
    display: grid;
    grid-template-columns: minmax(0, 1.15fr) minmax(0, 0.85fr);
    gap: 56px;
    align-items: start;
    padding-bottom: 56px;

    @media (max-width: 980px) {
      grid-template-columns: 1fr;
      gap: 40px;
    }
  }

  &__main {
    min-width: 0;
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
    background: rgba($bg-card, 0.7);
  }

  .pulse-dot {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: $status-ok;
    box-shadow: 0 0 0 0 rgba($status-ok, 0.5);
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
    font-size: clamp(2.2rem, 4.2vw, 3.35rem);
    font-weight: 800;
    line-height: 1;
    margin-bottom: 22px;
    letter-spacing: -0.04em;
  }

  &__firstname,
  &__lastname {
    display: block;
  }

  &__lastname {
    background: $gradient-hero;
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  }

  &__role {
    font-size: clamp(1.02rem, 2vw, 1.16rem);
    color: $text-secondary;
    max-width: 540px;
    line-height: 1.75;
    margin-bottom: 22px;

    em {
      font-style: normal;
      color: $text-primary;
      font-weight: 700;
    }
  }

  &__chips {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 28px;

    span {
      font-family: $font-mono;
      font-size: 0.7rem;
      letter-spacing: 0.04em;
      padding: 6px 11px;
      border-radius: 6px;
      border: 1px solid $border-color;
      color: $text-secondary;
      background: rgba($bg-card, 0.6);
    }
  }

  &__actions {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
    margin-bottom: 40px;
  }

  &__facts {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 20px;
    padding-top: 24px;
    border-top: 1px solid $border-color;
    max-width: 520px;

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
      font-size: 1.05rem;
      font-weight: 700;
    }
  }

  &__aside {
    width: 100%;
  }

  &__board {
    padding: 22px;
    background: linear-gradient(180deg, rgba($bg-card, 0.95), rgba($bg-secondary, 0.88));
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    margin-bottom: 22px;
    box-shadow: $shadow-card;
  }

  &__board-label {
    font-family: $font-mono;
    font-size: 0.68rem;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: $text-muted;
    margin-bottom: 16px;
  }

  &__layers {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  &__layer {
    h3 {
      font-family: $font-mono;
      font-size: 0.68rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      font-weight: 500;
      margin-bottom: 10px;
      color: $accent;
    }

    &:nth-child(2) h3 { color: $accent-blue; }
    &:nth-child(3) h3 { color: $accent-soft; }

    ul {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
    }

    li {
      font-size: 0.78rem;
      color: $text-secondary;
      padding: 5px 10px;
      background: rgba($text-primary, 0.03);
      border-radius: 6px;
      border: 1px solid $border-color;
    }
  }

  &__aside-label {
    font-family: $font-mono;
    font-size: 0.68rem;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: $text-muted;
    margin-bottom: 12px;
  }

  &__works {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  &__work {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 14px 16px;
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
      font-size: 0.95rem;
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

  &__marquee {
    position: relative;
    z-index: 1;
    overflow: hidden;
    border-top: 1px solid $border-color;
    border-bottom: 1px solid $border-color;
    padding: 14px 0;
    margin-top: auto;
    mask-image: linear-gradient(90deg, transparent, #000 8%, #000 92%, transparent);
  }

  &__track {
    display: flex;
    gap: 40px;
    width: max-content;
    animation: marquee 32s linear infinite;

    span {
      font-family: $font-mono;
      font-size: 0.78rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: $text-muted;
      white-space: nowrap;
    }
  }

  &__scroll {
    display: none;
  }
}

@keyframes pulse {
  0%   { box-shadow: 0 0 0 0 rgba($status-ok, 0.5); }
  70%  { box-shadow: 0 0 0 8px rgba($status-ok, 0); }
  100% { box-shadow: 0 0 0 0 rgba($status-ok, 0); }
}

@keyframes bounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50%       { transform: translateX(-50%) translateY(7px); }
}

@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}

@media (prefers-reduced-motion: reduce) {
  .hero__track { animation: none; }
}
</style>
