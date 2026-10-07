<script setup>
import { ArrowUp, Mail, MapPin, Phone } from 'lucide-vue-next'

const navLinks = [
  { id: 'hero', label: 'Accueil' },
  { id: 'about', label: 'À propos' },
  { id: 'projects', label: 'Projets' },
  { id: 'contact', label: 'Contact' },
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
    <div class="container footer__inner">
      <div>
        <a class="footer__logo" href="#" @click.prevent="scrollTop()">
          <span>N</span> Néhémie Pédahel Kouyo
        </a>
        <p>Développeur Full-Stack · Abidjan</p>
        <div class="footer__contacts">
          <a href="mailto:kouyonehemiepedahel@gmail.com"><Mail :size="14" /> kouyonehemiepedahel@gmail.com</a>
          <a href="tel:+2250779951800"><Phone :size="14" /> +225 07 79 951 800</a>
          <span><MapPin :size="14" /> Abidjan, Côte d'Ivoire</span>
        </div>
      </div>

      <div>
        <h4>Navigation</h4>
        <ul>
          <li v-for="link in navLinks" :key="link.id">
            <a href="#" @click.prevent="scrollTo(link.id)">{{ link.label }}</a>
          </li>
        </ul>
      </div>

      <div>
        <h4>Stack</h4>
        <div class="footer__tags">
          <span v-for="t in ['Angular', 'Vue.js', 'Flutter', 'Spring Boot', 'Laravel', 'PostgreSQL', 'Docker']" :key="t">{{ t }}</span>
        </div>
      </div>
    </div>

    <div class="footer__bottom">
      <div class="container footer__bottom-inner">
        <p>© {{ year }} Néhémie Pédahel Kouyo</p>
        <button @click="scrollTop" title="Retour en haut"><ArrowUp :size="16" /></button>
      </div>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.footer {
  background: $bg-secondary;
  border-top: 1px solid $border-color;

  &__inner {
    display: grid;
    grid-template-columns: 1.4fr 0.8fr 1fr;
    gap: 48px;
    padding-top: 56px;
    padding-bottom: 40px;

    @media (max-width: 800px) {
      grid-template-columns: 1fr;
      gap: 32px;
    }

    p {
      color: $text-secondary;
      margin: 12px 0 20px;
      font-size: 0.92rem;
    }

    h4 {
      font-family: $font-mono;
      font-size: 0.7rem;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: $text-muted;
      font-weight: 500;
      margin-bottom: 16px;
    }

    ul {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    a {
      color: $text-secondary;
      font-size: 0.9rem;
      &:hover { color: $accent; }
    }
  }

  &__logo {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    font-family: $font-heading;
    font-weight: 700;
    color: $text-primary !important;

    span {
      width: 28px;
      height: 28px;
      border-radius: 7px;
      background: $accent;
      color: $ink;
      display: grid;
      place-items: center;
      font-size: 0.85rem;
    }
  }

  &__contacts {
    display: flex;
    flex-direction: column;
    gap: 8px;

    a, span {
      display: flex;
      align-items: center;
      gap: 8px;
      font-size: 0.82rem;
      color: $text-muted;
    }
  }

  &__tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;

    span {
      font-family: $font-mono;
      font-size: 0.7rem;
      padding: 4px 10px;
      border-radius: 100px;
      border: 1px solid $border-color;
      color: $text-muted;
    }
  }

  &__bottom {
    border-top: 1px solid $border-color;
  }

  &__bottom-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 18px 0;

    p { font-size: 0.8rem; color: $text-muted; }

    button {
      width: 36px;
      height: 36px;
      border-radius: 8px;
      border: 1px solid rgba($accent, 0.25);
      color: $accent;
      display: grid;
      place-items: center;

      &:hover {
        background: $accent;
        color: $ink;
      }
    }
  }
}
</style>
