<script setup>
import { Mail, Phone, MapPin, Heart, ArrowUp } from 'lucide-vue-next'

const navLinks = [
  { id: 'hero',       label: 'Accueil' },
  { id: 'about',      label: 'À propos' },
  { id: 'skills',     label: 'Compétences' },
  { id: 'experience', label: 'Expérience' },
  { id: 'projects',   label: 'Projets' },
  { id: 'education',  label: 'Formation' },
  { id: 'contact',    label: 'Contact' },
]

function scrollTo(id) {
  const el = document.getElementById(id)
  if (el) window.scrollTo({ top: el.getBoundingClientRect().top + window.scrollY - 80, behavior: 'smooth' })
}

function scrollTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

const year = new Date().getFullYear()
</script>

<template>
  <footer class="footer">
    <div class="footer__top">
      <div class="footer__blob"></div>
    </div>

    <div class="container footer__inner">
      <!-- Brand -->
      <div class="footer__brand">
        <a class="footer__logo" @click.prevent="scrollTop()" href="#">
          <span class="logo-bracket">&lt;</span>
          <span class="logo-name">Néhémie</span>
          <span class="logo-bracket">/&gt;</span>
        </a>
        <p class="footer__tagline">Développeur Web &amp; Mobile<br/>basé à Abidjan, Côte d'Ivoire</p>

        <div class="footer__contact-list">
          <a href="mailto:kouyonehemiepedahel@gmail.com" class="footer__contact-item">
            <Mail :size="15" /> kouyonehemiepedahel@gmail.com
          </a>
          <a href="tel:+2250779951800" class="footer__contact-item">
            <Phone :size="15" /> +225 07-79-951-800
          </a>
          <span class="footer__contact-item static">
            <MapPin :size="15" /> Abidjan, Côte d'Ivoire
          </span>
        </div>
      </div>

      <!-- Nav links -->
      <div class="footer__nav">
        <h4 class="footer__nav-title">Navigation</h4>
        <ul>
          <li v-for="link in navLinks" :key="link.id">
            <a href="#" class="footer__nav-link" @click.prevent="scrollTo(link.id)">{{ link.label }}</a>
          </li>
        </ul>
      </div>

      <!-- Stack -->
      <div class="footer__stack">
        <h4 class="footer__nav-title">Stack technique</h4>
        <div class="footer__tags">
          <span v-for="t in ['Angular','Vue.js','Flutter','Spring Boot','Laravel','Docker','TypeScript','PostgreSQL']" :key="t" class="footer__tag">{{ t }}</span>
        </div>
      </div>
    </div>

    <!-- Bottom bar -->
    <div class="footer__bottom">
      <div class="container footer__bottom-inner">
        <p class="footer__copy">
          &copy; {{ year }} Néhémie Pédahel Kouyo — Conçu avec
          <Heart :size="13" class="footer__heart" /> à Abidjan
        </p>
        <button class="footer__back-top" @click="scrollTop" title="Retour en haut">
          <ArrowUp :size="18" />
        </button>
      </div>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.footer {
  position: relative;
  background: $bg-secondary;
  border-top: 1px solid $border-color;
  overflow: hidden;

  &__top {
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
    background: $gradient-main;
    opacity: 0.6;
  }

  &__blob {
    position: absolute;
    width: 400px; height: 400px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba($accent-violet, 0.06), transparent 70%);
    top: -200px; left: 50%;
    transform: translateX(-50%);
    pointer-events: none;
  }

  &__inner {
    position: relative;
    display: grid;
    grid-template-columns: 1.4fr 1fr 1fr;
    gap: 60px;
    padding-top: 64px;
    padding-bottom: 48px;

    @media (max-width: 900px) {
      grid-template-columns: 1fr 1fr;
      gap: 40px;
    }

    @media (max-width: 600px) {
      grid-template-columns: 1fr;
      gap: 36px;
    }
  }

  // ── Brand ─────────────────────────────────────────────────
  &__logo {
    font-family: $font-mono;
    font-size: 1.2rem;
    font-weight: 500;
    display: inline-block;
    margin-bottom: 14px;
    cursor: pointer;

    .logo-bracket { color: $accent-blue; }
    .logo-name    { color: $text-primary; font-weight: 700; }
  }

  &__tagline {
    font-size: 0.9rem;
    color: $text-secondary;
    line-height: 1.7;
    margin-bottom: 24px;
  }

  &__contact-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  &__contact-item {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 0.83rem;
    color: $text-muted;
    transition: $transition-fast;

    &:not(.static):hover { color: $accent-blue; }

    &.static { cursor: default; }
  }

  // ── Nav ───────────────────────────────────────────────────
  &__nav-title {
    font-size: 0.78rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: $text-muted;
    font-family: $font-mono;
    margin-bottom: 20px;
  }

  ul {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  &__nav-link {
    font-size: 0.9rem;
    color: $text-secondary;
    transition: $transition-fast;

    &:hover {
      color: $accent-blue;
      padding-left: 4px;
    }
  }

  // ── Stack tags ────────────────────────────────────────────
  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
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
    transition: $transition-fast;

    &:hover {
      background: rgba($accent-blue, 0.16);
    }
  }

  // ── Bottom bar ────────────────────────────────────────────
  &__bottom {
    border-top: 1px solid $border-color;
  }

  &__bottom-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 20px;
    padding-bottom: 20px;
    gap: 16px;
  }

  &__copy {
    font-size: 0.83rem;
    color: $text-muted;
    display: flex;
    align-items: center;
    gap: 6px;
    flex-wrap: wrap;
  }

  &__heart {
    color: $accent-pink;
    animation: heartbeat 1.5s ease-in-out infinite;
    flex-shrink: 0;
  }

  &__back-top {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 38px; height: 38px;
    border-radius: $radius-sm;
    background: rgba($accent-blue, 0.1);
    border: 1px solid rgba($accent-blue, 0.2);
    color: $accent-blue;
    cursor: pointer;
    transition: $transition-base;

    &:hover {
      background: $gradient-main;
      border-color: transparent;
      color: #fff;
      transform: translateY(-3px);
    }
  }
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  20%, 60%  { transform: scale(1.2); }
  40%, 80%  { transform: scale(1); }
}
</style>
