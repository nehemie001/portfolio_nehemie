<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { ArrowDown, Mail, Phone, Github, Linkedin, MapPin } from 'lucide-vue-next'

const titles = [
  'Développeur Web Angular',
  'Développeur Mobile Flutter',
  'Développeur Full-Stack',
  'Architecte Microservices',
]

const displayedTitle = ref('')
const titleIndex = ref(0)
const charIndex = ref(0)
const isDeleting = ref(false)
let timer = null

function typeEffect() {
  const current = titles[titleIndex.value]
  if (!isDeleting.value) {
    displayedTitle.value = current.slice(0, charIndex.value + 1)
    charIndex.value++
    if (charIndex.value === current.length) {
      isDeleting.value = true
      timer = setTimeout(typeEffect, 1800)
      return
    }
    timer = setTimeout(typeEffect, 60)
  } else {
    displayedTitle.value = current.slice(0, charIndex.value - 1)
    charIndex.value--
    if (charIndex.value === 0) {
      isDeleting.value = false
      titleIndex.value = (titleIndex.value + 1) % titles.length
      timer = setTimeout(typeEffect, 300)
      return
    }
    timer = setTimeout(typeEffect, 35)
  }
}

function scrollToAbout() {
  const el = document.getElementById('about')
  if (el) window.scrollTo({ top: el.getBoundingClientRect().top + window.scrollY - 80, behavior: 'smooth' })
}

onMounted(() => { timer = setTimeout(typeEffect, 600) })
onUnmounted(() => clearTimeout(timer))
</script>

<template>
  <section id="hero" class="hero">
    <!-- Background elements -->
    <div class="hero__bg">
      <div class="hero__blob hero__blob--1"></div>
      <div class="hero__blob hero__blob--2"></div>
      <div class="hero__blob hero__blob--3"></div>
      <div class="hero__grid"></div>
    </div>

    <div class="container hero__content">
      <!-- Badge -->
      <div class="hero__badge">
        <span class="pulse-dot"></span>
        <MapPin :size="13" />
        Abidjan, Côte d'Ivoire
      </div>

      <!-- Name -->
      <h1 class="hero__name">
        <span class="hero__hello">Bonjour, je suis</span>
        <span class="hero__firstname">Néhémie</span>
        <span class="hero__lastname">Pédahel Kouyo</span>
      </h1>

      <!-- Typing title -->
      <div class="hero__typing">
        <span class="hero__typing-prefix">&gt;&nbsp;</span>
        <span class="hero__typing-text">{{ displayedTitle }}</span>
        <span class="hero__cursor">|</span>
      </div>

      <!-- Description -->
      <p class="hero__desc">
        Passionné par la création d'expériences digitales performantes et élégantes.
        Je conçois des applications web et mobiles robustes, de la maquette Figma
        jusqu'au déploiement en production.
      </p>

      <!-- CTAs -->
      <div class="hero__actions">
        <a href="mailto:kouyonehemiepedahel@gmail.com" class="btn-primary">
          <Mail :size="18" /> Travaillons ensemble
        </a>
        <button class="btn-outline" @click="scrollToAbout">
          Découvrir mon profil
        </button>
      </div>

      <!-- Social links -->
      <div class="hero__socials">
        <a href="mailto:kouyonehemiepedahel@gmail.com" class="hero__social" title="Email">
          <Mail :size="18" />
        </a>
        <a href="tel:+2250779951800" class="hero__social" title="Téléphone">
          <Phone :size="18" />
        </a>
        <a href="#" class="hero__social" title="GitHub">
          <Github :size="18" />
        </a>
        <a href="#" class="hero__social" title="LinkedIn">
          <Linkedin :size="18" />
        </a>
      </div>
    </div>

    <!-- Scroll arrow -->
    <button class="hero__scroll-arrow" @click="scrollToAbout">
      <ArrowDown :size="22" />
    </button>

    <!-- Floating tech badges -->
    <div class="hero__float hero__float--1">Flutter</div>
    <div class="hero__float hero__float--2">Angular</div>
    <div class="hero__float hero__float--3">Vue.js</div>
    <div class="hero__float hero__float--4">Spring Boot</div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  overflow: hidden;
  padding: 120px 0 80px;

  // ── Background ──────────────────────────────────────────
  &__bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
    z-index: 0;
  }

  &__blob {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.18;
    animation: floatBlob 10s ease-in-out infinite;

    &--1 {
      width: 600px; height: 600px;
      background: $accent-blue;
      top: -200px; right: -150px;
      animation-delay: 0s;
    }
    &--2 {
      width: 500px; height: 500px;
      background: $accent-violet;
      bottom: -150px; left: -100px;
      animation-delay: -3s;
    }
    &--3 {
      width: 300px; height: 300px;
      background: $accent-cyan;
      top: 40%; left: 50%;
      opacity: 0.1;
      animation-delay: -6s;
    }
  }

  &__grid {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(rgba(99, 102, 241, 0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(99, 102, 241, 0.04) 1px, transparent 1px);
    background-size: 60px 60px;
    mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 40%, transparent 100%);
  }

  // ── Content ─────────────────────────────────────────────
  &__content {
    position: relative;
    z-index: 1;
    max-width: 800px;
  }

  &__badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 18px;
    background: rgba($accent-blue, 0.1);
    border: 1px solid rgba($accent-blue, 0.2);
    border-radius: 100px;
    font-size: 0.82rem;
    font-weight: 500;
    color: $accent-blue;
    margin-bottom: 28px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.1s;
    opacity: 0;
  }

  .pulse-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: #22c55e;
    box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.5);
    animation: pulse 2s infinite;
  }

  &__name {
    margin-bottom: 20px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.2s;
    opacity: 0;
  }

  &__hello {
    display: block;
    font-size: clamp(1rem, 2.5vw, 1.25rem);
    font-weight: 400;
    color: $text-secondary;
    font-family: $font-mono;
    margin-bottom: 8px;
  }

  &__firstname {
    display: block;
    font-size: clamp(3rem, 8vw, 5.5rem);
    font-weight: 900;
    background: $gradient-hero;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
    margin-bottom: 4px;
  }

  &__lastname {
    display: block;
    font-size: clamp(1.6rem, 4vw, 2.8rem);
    font-weight: 700;
    color: $text-primary;
    line-height: 1.2;
  }

  &__typing {
    display: flex;
    align-items: center;
    gap: 2px;
    margin-bottom: 28px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.35s;
    opacity: 0;
  }

  &__typing-prefix {
    font-family: $font-mono;
    font-size: clamp(1.1rem, 3vw, 1.5rem);
    color: $accent-cyan;
    font-weight: 500;
  }

  &__typing-text {
    font-family: $font-mono;
    font-size: clamp(1.1rem, 3vw, 1.5rem);
    font-weight: 600;
    background: $gradient-main;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  &__cursor {
    font-family: $font-mono;
    font-size: clamp(1.1rem, 3vw, 1.5rem);
    color: $accent-blue;
    animation: blink 1s step-end infinite;
    font-weight: 300;
  }

  &__desc {
    font-size: clamp(1rem, 2vw, 1.1rem);
    color: $text-secondary;
    max-width: 580px;
    line-height: 1.9;
    margin-bottom: 40px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.5s;
    opacity: 0;
  }

  &__actions {
    display: flex;
    align-items: center;
    gap: 16px;
    flex-wrap: wrap;
    margin-bottom: 48px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.65s;
    opacity: 0;
  }

  &__socials {
    display: flex;
    align-items: center;
    gap: 12px;
    animation: fadeSlideDown 0.8s ease both;
    animation-delay: 0.8s;
    opacity: 0;
  }

  &__social {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 44px; height: 44px;
    border-radius: $radius-sm;
    background: rgba(255,255,255,0.04);
    border: 1px solid $border-color;
    color: $text-secondary;
    transition: $transition-base;

    &:hover {
      border-color: $accent-blue;
      color: $accent-blue;
      background: rgba($accent-blue, 0.1);
      transform: translateY(-2px);
    }
  }

  // ── Scroll arrow ─────────────────────────────────────────
  &__scroll-arrow {
    position: absolute;
    bottom: 36px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    align-items: center;
    justify-content: center;
    width: 44px; height: 44px;
    border-radius: 50%;
    background: rgba(255,255,255,0.04);
    border: 1px solid $border-color;
    color: $text-muted;
    transition: $transition-base;
    animation: bounce 2.5s ease-in-out infinite;
    z-index: 1;

    &:hover {
      border-color: $accent-blue;
      color: $accent-blue;
    }
  }

  // ── Floating tech badges ─────────────────────────────────
  &__float {
    position: absolute;
    font-family: $font-mono;
    font-size: 0.78rem;
    font-weight: 500;
    padding: 8px 16px;
    border-radius: 100px;
    background: rgba($bg-card, 0.9);
    border: 1px solid $border-color;
    color: $text-secondary;
    pointer-events: none;
    animation: floatBadge 6s ease-in-out infinite;
    z-index: 0;
    backdrop-filter: blur(10px);

    @media (max-width: 768px) { display: none; }

    &--1 { top: 25%; right: 8%; animation-delay: 0s; }
    &--2 { top: 45%; right: 12%; animation-delay: -2s; }
    &--3 { top: 62%; right: 5%; animation-delay: -4s; }
    &--4 { top: 35%; right: 22%; animation-delay: -1s; }
  }
}

// ── Keyframes ────────────────────────────────────────────────
@keyframes fadeSlideDown {
  from { opacity: 0; transform: translateY(-20px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0; }
}

@keyframes pulse {
  0%   { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.5); }
  70%  { box-shadow: 0 0 0 8px rgba(34, 197, 94, 0); }
  100% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0); }
}

@keyframes bounce {
  0%, 100% { transform: translateX(-50%) translateY(0); }
  50%       { transform: translateX(-50%) translateY(8px); }
}

@keyframes floatBlob {
  0%, 100% { transform: translate(0, 0) scale(1); }
  33%       { transform: translate(30px, -20px) scale(1.05); }
  66%       { transform: translate(-20px, 20px) scale(0.95); }
}

@keyframes floatBadge {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-10px); }
}
</style>
