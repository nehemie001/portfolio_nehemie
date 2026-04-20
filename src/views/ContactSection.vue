<script setup>
import { ref, onMounted } from 'vue'
import { Mail, Phone, MapPin, Send, Copy, Check } from 'lucide-vue-next'

const sectionRef = ref(null)
const copiedEmail = ref(false)
const copiedPhone = ref(false)

const form = ref({ name: '', email: '', subject: '', message: '' })
const sending = ref(false)
const sent = ref(false)

function copyText(text, which) {
  navigator.clipboard.writeText(text).then(() => {
    if (which === 'email') {
      copiedEmail.value = true
      setTimeout(() => copiedEmail.value = false, 2000)
    } else {
      copiedPhone.value = true
      setTimeout(() => copiedPhone.value = false, 2000)
    }
  })
}

async function handleSubmit() {
  sending.value = true
  await new Promise(r => setTimeout(r, 1400))
  sending.value = false
  sent.value = true
  form.value = { name: '', email: '', subject: '', message: '' }
  setTimeout(() => sent.value = false, 4000)
}

const contacts = [
  {
    label: 'Email',
    value: 'kouyonehemiepedahel@gmail.com',
    href: 'mailto:kouyonehemiepedahel@gmail.com',
    icon: Mail,
    color: '#6366f1',
    copyKey: 'email',
  },
  {
    label: 'Téléphone',
    value: '+225 07-79-951-800',
    href: 'tel:+2250779951800',
    icon: Phone,
    color: '#8b5cf6',
    copyKey: 'phone',
  },
  {
    label: 'Localisation',
    value: 'Abidjan, Côte d\'Ivoire',
    href: null,
    icon: MapPin,
    color: '#22d3ee',
    copyKey: null,
  },
]

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => entries.forEach(e => {
      if (e.isIntersecting) e.target.classList.add('visible')
    }),
    { threshold: 0.1 }
  )
  sectionRef.value?.querySelectorAll('.reveal, .reveal-left, .reveal-right').forEach(el => observer.observe(el))
})
</script>

<template>
  <section id="contact" class="section contact" ref="sectionRef">
    <div class="container">
      <div class="section-header reveal">
        <span class="section-label">// Contact</span>
        <h2>Travaillons ensemble</h2>
        <p>Vous avez un projet ? Une opportunité ? Je suis disponible et à l'écoute.</p>
      </div>

      <div class="contact__grid">
        <!-- Left: info -->
        <div class="contact__info reveal-left">
          <p class="contact__intro">
            N'hésitez pas à me contacter pour discuter de vos projets web ou mobile,
            d'une opportunité professionnelle, ou simplement pour échanger.
            Je réponds généralement dans les <strong>24 heures</strong>.
          </p>

          <div class="contact__items">
            <div
              v-for="c in contacts"
              :key="c.label"
              class="contact__item"
              :style="`--c: ${c.color}`"
            >
              <div class="contact__item-icon">
                <component :is="c.icon" :size="20" />
              </div>
              <div class="contact__item-body">
                <span class="contact__item-label">{{ c.label }}</span>
                <a v-if="c.href" :href="c.href" class="contact__item-value">{{ c.value }}</a>
                <span v-else class="contact__item-value">{{ c.value }}</span>
              </div>
              <button
                v-if="c.copyKey"
                class="contact__copy-btn"
                @click="copyText(c.value, c.copyKey)"
                :title="`Copier ${c.label}`"
              >
                <Check v-if="(c.copyKey === 'email' && copiedEmail) || (c.copyKey === 'phone' && copiedPhone)" :size="15" style="color: #22c55e" />
                <Copy v-else :size="15" />
              </button>
            </div>
          </div>

          <!-- Second phone -->
          <div class="contact__second-phone">
            <Phone :size="14" />
            <span>+225 07-59-837-456</span>
          </div>

          <!-- Availability badge -->
          <div class="contact__availability">
            <span class="pulse-green"></span>
            Disponible pour de nouvelles opportunités
          </div>
        </div>

        <!-- Right: form -->
        <div class="contact__form-wrap reveal-right">
          <form class="contact__form" @submit.prevent="handleSubmit">
            <div class="contact__form-row">
              <div class="contact__field">
                <label for="cf-name">Votre nom</label>
                <input id="cf-name" v-model="form.name" type="text" placeholder="Jean Dupont" required />
              </div>
              <div class="contact__field">
                <label for="cf-email">Votre email</label>
                <input id="cf-email" v-model="form.email" type="email" placeholder="jean@example.com" required />
              </div>
            </div>
            <div class="contact__field">
              <label for="cf-subject">Sujet</label>
              <input id="cf-subject" v-model="form.subject" type="text" placeholder="Proposition de projet, opportunité..." required />
            </div>
            <div class="contact__field">
              <label for="cf-message">Message</label>
              <textarea id="cf-message" v-model="form.message" rows="5" placeholder="Décrivez votre projet ou votre demande..." required></textarea>
            </div>

            <button type="submit" class="btn-primary contact__submit" :disabled="sending || sent">
              <template v-if="sent">
                <Check :size="18" /> Message envoyé !
              </template>
              <template v-else-if="sending">
                <span class="spinner"></span> Envoi en cours…
              </template>
              <template v-else>
                <Send :size="18" /> Envoyer le message
              </template>
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/main.scss' as *;

.contact {
  background: $bg-primary;

  &__grid {
    display: grid;
    grid-template-columns: 1fr 1.3fr;
    gap: 64px;
    align-items: start;

    @media (max-width: 900px) {
      grid-template-columns: 1fr;
      gap: 48px;
    }
  }

  &__intro {
    font-size: 0.97rem;
    color: $text-secondary;
    line-height: 1.9;
    margin-bottom: 36px;

    strong { color: $accent-blue; font-weight: 700; }
  }

  &__items {
    display: flex;
    flex-direction: column;
    gap: 14px;
    margin-bottom: 20px;
  }

  &__item {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px 18px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
    transition: $transition-base;

    &:hover {
      border-color: var(--c, $accent-blue);
      background: $bg-card-hover;
    }
  }

  &__item-icon {
    flex-shrink: 0;
    width: 42px; height: 42px;
    border-radius: $radius-sm;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--c, $accent-blue);
    background: rgba(var(--c, #6366f1), 0.1);
    background: color-mix(in srgb, var(--c, $accent-blue) 12%, transparent);
    border: 1px solid rgba(255,255,255,0.05);
  }

  &__item-body {
    display: flex;
    flex-direction: column;
    gap: 2px;
    flex: 1;
    min-width: 0;
  }

  &__item-label {
    font-size: 0.72rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: $text-muted;
    font-family: $font-mono;
  }

  &__item-value {
    font-size: 0.9rem;
    font-weight: 600;
    color: $text-primary;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    transition: $transition-fast;

    &:hover { color: var(--c, $accent-blue); }
  }

  &__copy-btn {
    flex-shrink: 0;
    width: 32px; height: 32px;
    border-radius: $radius-sm;
    display: flex;
    align-items: center;
    justify-content: center;
    color: $text-muted;
    background: rgba(255,255,255,0.04);
    border: 1px solid $border-color;
    cursor: pointer;
    transition: $transition-fast;

    &:hover {
      color: var(--c, $accent-blue);
      border-color: var(--c, $accent-blue);
    }
  }

  &__second-phone {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.85rem;
    color: $text-muted;
    padding-left: 4px;
    margin-bottom: 24px;
  }

  &__availability {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 12px 20px;
    background: rgba(34, 197, 94, 0.08);
    border: 1px solid rgba(34, 197, 94, 0.2);
    border-radius: 100px;
    font-size: 0.85rem;
    font-weight: 500;
    color: #22c55e;
  }

  .pulse-green {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: #22c55e;
    box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.5);
    animation: pulseG 2s infinite;
    flex-shrink: 0;
  }

  // ── Form ──────────────────────────────────────────────────
  &__form-wrap {
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-xl;
    padding: 36px 32px;

    @media (max-width: 640px) {
      padding: 24px 20px;
    }
  }

  &__form {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  &__form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;

    @media (max-width: 540px) {
      grid-template-columns: 1fr;
    }
  }

  &__field {
    display: flex;
    flex-direction: column;
    gap: 8px;

    label {
      font-size: 0.82rem;
      font-weight: 600;
      color: $text-secondary;
      letter-spacing: 0.02em;
    }

    input, textarea {
      font-family: $font-body;
      font-size: 0.92rem;
      color: $text-primary;
      background: rgba(255,255,255,0.04);
      border: 1px solid $border-color;
      border-radius: $radius-sm;
      padding: 12px 16px;
      outline: none;
      transition: $transition-fast;
      resize: vertical;

      &::placeholder { color: $text-muted; }

      &:focus {
        border-color: $accent-blue;
        background: rgba($accent-blue, 0.05);
        box-shadow: 0 0 0 3px rgba($accent-blue, 0.1);
      }
    }
  }

  &__submit {
    width: 100%;
    justify-content: center;

    &:disabled {
      opacity: 0.75;
      cursor: not-allowed;
      transform: none !important;
    }
  }
}

.spinner {
  width: 16px; height: 16px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin  { to { transform: rotate(360deg); } }
@keyframes pulseG {
  0%   { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0.5); }
  70%  { box-shadow: 0 0 0 8px rgba(34, 197, 94, 0); }
  100% { box-shadow: 0 0 0 0 rgba(34, 197, 94, 0); }
}
</style>
