<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Menu, X } from 'lucide-vue-next'

const isScrolled = ref(false)
const mobileOpen = ref(false)
const activeSection = ref('hero')
const progress = ref(0)

const navLinks = [
  { id: 'about',      label: 'À propos' },
  { id: 'skills',     label: 'Compétences' },
  { id: 'experience', label: 'Expérience' },
  { id: 'projects',   label: 'Projets' },
  { id: 'education',  label: 'Formation' },
  { id: 'contact',    label: 'Contact' },
]

function handleScroll() {
  isScrolled.value = window.scrollY > 16
  const doc = document.documentElement
  const max = doc.scrollHeight - window.innerHeight
  progress.value = max > 0 ? (window.scrollY / max) * 100 : 0

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
  <nav class="navbar" :class="{ scrolled: isScrolled, 'mobile-open': mobileOpen }">
    <div class="navbar__progress" :style="{ width: progress + '%' }"></div>
    <div class="navbar__inner container">
      <a class="navbar__logo" @click.prevent="scrollTo('hero')" href="#hero">
        <span class="logo-mark">N</span>
        <span class="logo-name">Néhémie</span>
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
        Me contacter
      </a>

      <button class="navbar__burger" @click="mobileOpen = !mobileOpen" aria-label="Menu">
        <X v-if="mobileOpen" :size="22" />
        <Menu v-else :size="22" />
      </button>
    </div>

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
        Me contacter
      </a>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  transition: $transition-base;
  padding: 18px 0;

  &__progress {
    position: absolute;
    top: 0;
    left: 0;
    height: 2px;
    background: $accent;
    width: 0;
    transition: width 0.1s linear;
  }

  &.scrolled {
    background: rgba($bg-primary, 0.82);
    backdrop-filter: blur(22px);
    -webkit-backdrop-filter: blur(22px);
    border-bottom: 1px solid $border-color;
    padding: 10px 0;
  }

  &__inner {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  &__logo {
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
    white-space: nowrap;
    flex-shrink: 0;
  }

  .logo-mark {
    width: 32px;
    height: 32px;
    border-radius: 8px;
    background: $accent;
    color: $ink;
    font-family: $font-heading;
    font-weight: 800;
    font-size: 0.95rem;
    display: grid;
    place-items: center;
    box-shadow: 4px 4px 0 0 $accent-blue;
  }

  .logo-name {
    font-family: $font-heading;
    font-weight: 700;
    font-size: 1.02rem;
    letter-spacing: -0.03em;
  }

  &__links {
    display: flex;
    align-items: center;
    gap: 0;
    flex: 1;
    justify-content: center;
    min-width: 0;

    @media (max-width: 1180px) { display: none; }
  }

  &__link {
    padding: 6px 9px;
    font-size: 0.78rem;
    font-weight: 500;
    color: $text-secondary;
    border-radius: 6px;
    transition: $transition-fast;

    &:hover, &.active {
      color: $text-primary;
    }

    &.active {
      color: $accent;
      background: rgba($accent, 0.08);
    }
  }

  &__cta {
    flex-shrink: 0;
    font-size: 0.82rem;
    padding: 9px 18px;

    @media (max-width: 1180px) { display: none; }
  }

  &__burger {
    display: none;
    color: $text-primary;
    padding: 6px;
    border-radius: $radius-sm;
    margin-left: auto;

    &:hover { background: rgba(255, 255, 255, 0.06); }

    @media (max-width: 1180px) { display: flex; }
  }

  &__mobile {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    flex-direction: column;
    gap: 8px;
    padding: 20px $container-pad 28px;
    background: $bg-secondary;
    border-bottom: 1px solid $border-color;
    box-shadow: $shadow-card;
    transform: translateY(-8px);
    opacity: 0;
    visibility: hidden;
    pointer-events: none;
    transition: $transition-base;

    @media (max-width: 1180px) { display: flex; }

    &.open {
      transform: translateY(0);
      opacity: 1;
      visibility: visible;
      pointer-events: auto;
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

    &:hover, &.active {
      color: $accent;
      background: rgba($accent, 0.08);
    }
  }
}

.mt-4 { margin-top: 16px; align-self: flex-start; }
</style>
