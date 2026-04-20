<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Menu, X } from 'lucide-vue-next'

const isScrolled = ref(false)
const mobileOpen = ref(false)
const activeSection = ref('hero')

const navLinks = [
  { id: 'hero',       label: 'Accueil' },
  { id: 'about',      label: 'À propos' },
  { id: 'skills',     label: 'Compétences' },
  { id: 'experience', label: 'Expérience' },
  { id: 'projects',   label: 'Projets' },
  { id: 'education',  label: 'Formation' },
  { id: 'contact',    label: 'Contact' },
]

function handleScroll() {
  isScrolled.value = window.scrollY > 20

  const sections = navLinks.map(l => document.getElementById(l.id)).filter(Boolean)
  for (let i = sections.length - 1; i >= 0; i--) {
    const rect = sections[i].getBoundingClientRect()
    if (rect.top <= 120) {
      activeSection.value = sections[i].id
      break
    }
  }
}

function scrollTo(id) {
  mobileOpen.value = false
  const el = document.getElementById(id)
  if (el) {
    const top = el.getBoundingClientRect().top + window.scrollY - 80
    window.scrollTo({ top, behavior: 'smooth' })
  }
}

onMounted(() => window.addEventListener('scroll', handleScroll, { passive: true }))
onUnmounted(() => window.removeEventListener('scroll', handleScroll))
</script>

<template>
  <nav class="navbar" :class="{ 'scrolled': isScrolled, 'mobile-open': mobileOpen }">
    <div class="navbar__inner container">
      <a class="navbar__logo" @click.prevent="scrollTo('hero')" href="#hero">
        <span class="logo-bracket">&lt;</span>
        <span class="logo-name">Néhémie</span>
        <span class="logo-bracket">/&gt;</span>
      </a>

      <ul class="navbar__links">
        <li v-for="link in navLinks" :key="link.id">
          <a
            href="#"
            class="navbar__link"
            :class="{ active: activeSection === link.id }"
            @click.prevent="scrollTo(link.id)"
          >{{ link.label }}</a>
        </li>
      </ul>

      <a href="mailto:kouyonehemiepedahel@gmail.com" class="btn-primary navbar__cta">
        Contactez-moi
      </a>

      <button class="navbar__burger" @click="mobileOpen = !mobileOpen" aria-label="Menu">
        <X v-if="mobileOpen" :size="22" />
        <Menu v-else :size="22" />
      </button>
    </div>

    <!-- Mobile drawer -->
    <div class="navbar__mobile" :class="{ open: mobileOpen }">
      <ul>
        <li v-for="link in navLinks" :key="link.id">
          <a
            href="#"
            class="navbar__mobile-link"
            :class="{ active: activeSection === link.id }"
            @click.prevent="scrollTo(link.id)"
          >{{ link.label }}</a>
        </li>
      </ul>
      <a href="mailto:kouyonehemiepedahel@gmail.com" class="btn-primary mt-4">
        Contactez-moi
      </a>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  transition: $transition-base;
  padding: 18px 0;

  &.scrolled {
    background: rgba($bg-primary, 0.85);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-bottom: 1px solid $border-color;
    padding: 12px 0;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.3);
  }

  &__inner {
    display: flex;
    align-items: center;
    gap: 40px;
  }

  &__logo {
    font-family: $font-mono;
    font-size: 1.1rem;
    font-weight: 500;
    letter-spacing: -0.02em;
    cursor: pointer;
    white-space: nowrap;
    flex-shrink: 0;

    .logo-bracket {
      color: $accent-blue;
    }
    .logo-name {
      color: $text-primary;
      font-weight: 700;
    }
  }

  &__links {
    display: flex;
    align-items: center;
    gap: 4px;
    flex: 1;
    justify-content: center;

    @media (max-width: 900px) { display: none; }
  }

  &__link {
    padding: 7px 14px;
    font-size: 0.875rem;
    font-weight: 500;
    color: $text-secondary;
    border-radius: $radius-sm;
    transition: $transition-fast;
    position: relative;

    &::after {
      content: '';
      position: absolute;
      bottom: 2px;
      left: 50%;
      transform: translateX(-50%);
      width: 0;
      height: 2px;
      background: $gradient-main;
      border-radius: 1px;
      transition: $transition-base;
    }

    &:hover, &.active {
      color: $text-primary;

      &::after { width: 60%; }
    }

    &.active {
      color: $accent-blue;
    }
  }

  &__cta {
    flex-shrink: 0;
    font-size: 0.85rem;
    padding: 10px 22px;

    @media (max-width: 900px) { display: none; }
  }

  &__burger {
    display: none;
    color: $text-primary;
    padding: 6px;
    border-radius: $radius-sm;
    transition: $transition-fast;
    margin-left: auto;

    &:hover { background: rgba(255,255,255,0.06); }

    @media (max-width: 900px) { display: flex; }
  }

  &__mobile {
    display: none;
    flex-direction: column;
    gap: 8px;
    padding: 20px $container-pad 28px;
    background: rgba($bg-secondary, 0.97);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid $border-color;
    transform: translateY(-100%);
    opacity: 0;
    transition: $transition-base;

    @media (max-width: 900px) { display: flex; }

    &.open {
      transform: translateY(0);
      opacity: 1;
    }

    ul {
      display: flex;
      flex-direction: column;
      gap: 4px;
    }
  }

  &__mobile-link {
    display: block;
    padding: 12px 16px;
    font-size: 1rem;
    font-weight: 500;
    color: $text-secondary;
    border-radius: $radius-sm;
    transition: $transition-fast;

    &:hover, &.active {
      color: $accent-blue;
      background: rgba($accent-blue, 0.08);
    }
  }
}

.mt-4 { margin-top: 16px; align-self: flex-start; }
</style>
