<script setup>
import { onMounted, ref } from 'vue'
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
      setTimeout(() => { copiedEmail.value = false }, 2000)
    } else {
      copiedPhone.value = true
      setTimeout(() => { copiedPhone.value = false }, 2000)
    }
  })
}

async function handleSubmit() {
  sending.value = true
  const subject = encodeURIComponent(form.value.subject)
  const body = encodeURIComponent(
    `${form.value.message}\n\n— ${form.value.name} (${form.value.email})`
  )
  window.location.href = `mailto:kouyonehemiepedahel@gmail.com?subject=${subject}&body=${body}`
  await new Promise(r => setTimeout(r, 800))
  sending.value = false
  sent.value = true
  form.value = { name: '', email: '', subject: '', message: '' }
  setTimeout(() => { sent.value = false }, 4000)
}

const contacts = [
  {
    label: 'Email',
    value: 'kouyonehemiepedahel@gmail.com',
    href: 'mailto:kouyonehemiepedahel@gmail.com',
    icon: Mail,
    copyKey: 'email',
  },
  {
    label: 'Téléphone',
    value: '+225 07 79 951 800',
    href: 'tel:+2250779951800',
    icon: Phone,
    copyKey: 'phone',
  },
  {
    label: 'Localisation',
    value: 'Abidjan, Côte d\'Ivoire',
    href: null,
    icon: MapPin,
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
        <span class="section-label">06 — Contact</span>
        <h2>Un projet, une opportunité ?</h2>
        <p>Je réponds généralement sous 24 heures.</p>
      </div>

      <div class="contact__grid">
        <div class="contact__info reveal-left">
          <p>
            Web, mobile ou back-end — Angular, Vue.js, Flutter, Spring Boot, Laravel.
            Un produit à concevoir ou à faire grandir : écrivez-moi. On peut aussi simplement échanger.
          </p>

          <div class="contact__items">
            <div v-for="c in contacts" :key="c.label" class="contact__item">
              <div class="contact__icon">
                <component :is="c.icon" :size="18" />
              </div>
              <div class="contact__body">
                <span>{{ c.label }}</span>
                <a v-if="c.href" :href="c.href">{{ c.value }}</a>
                <strong v-else>{{ c.value }}</strong>
              </div>
              <button
                v-if="c.copyKey"
                class="contact__copy"
                @click="copyText(c.value, c.copyKey)"
                :title="`Copier ${c.label}`"
              >
                <Check
                  v-if="(c.copyKey === 'email' && copiedEmail) || (c.copyKey === 'phone' && copiedPhone)"
                  :size="15"
                  class="ok"
                />
                <Copy v-else :size="15" />
              </button>
            </div>
          </div>

          <p class="contact__alt">Aussi : +225 07 59 837 456</p>
          <div class="contact__avail"><i></i> Disponible pour de nouvelles opportunités</div>
        </div>

        <form class="contact__form reveal-right" @submit.prevent="handleSubmit">
          <div class="contact__row">
            <label>
              Votre nom
              <input v-model="form.name" type="text" placeholder="Jean Dupont" required />
            </label>
            <label>
              Votre email
              <input v-model="form.email" type="email" placeholder="jean@example.com" required />
            </label>
          </div>
          <label>
            Sujet
            <input v-model="form.subject" type="text" placeholder="Projet, opportunité…" required />
          </label>
          <label>
            Message
            <textarea v-model="form.message" rows="5" placeholder="Décrivez votre besoin…" required></textarea>
          </label>
          <button type="submit" class="btn-primary" :disabled="sending || sent">
            <template v-if="sent"><Check :size="18" /> Message préparé</template>
            <template v-else-if="sending"><span class="spinner"></span> Ouverture…</template>
            <template v-else><Send :size="16" /> Envoyer</template>
          </button>
        </form>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '../assets/tokens' as *;

.contact {
  background: $bg-primary;
  border-top: 1px solid $border-color;

  &__grid {
    display: grid;
    grid-template-columns: 0.9fr 1.1fr;
    gap: 56px;

    @media (max-width: 900px) {
      grid-template-columns: 1fr;
      gap: 40px;
    }
  }

  &__info p {
    color: $text-secondary;
    line-height: 1.8;
    margin-bottom: 28px;
  }

  &__items {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 16px;
  }

  &__item {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 14px 16px;
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-md;
  }

  &__icon {
    width: 38px;
    height: 38px;
    border-radius: 8px;
    display: grid;
    place-items: center;
    color: $accent;
    background: rgba($accent, 0.1);
    flex-shrink: 0;
  }

  &__body {
    flex: 1;
    min-width: 0;

    span {
      display: block;
      font-family: $font-mono;
      font-size: 0.66rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: $text-muted;
    }

    a, strong {
      font-size: 0.9rem;
      font-weight: 600;
      color: $text-primary;
    }

    a:hover { color: $accent; }
  }

  &__copy {
    width: 32px;
    height: 32px;
    border-radius: 8px;
    display: grid;
    place-items: center;
    color: $text-muted;
    border: 1px solid $border-color;

    &:hover { color: $accent; border-color: $accent; }
    .ok { color: $status-ok; }
  }

  &__alt {
    font-size: 0.84rem;
    color: $text-muted !important;
    margin-bottom: 20px !important;
  }

  &__avail {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 10px 16px;
    border-radius: 100px;
    background: rgba($status-ok, 0.08);
    border: 1px solid rgba($status-ok, 0.22);
    color: $status-ok;
    font-size: 0.84rem;
    font-weight: 500;

    i {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: $status-ok;
    }
  }

  &__form {
    background: $bg-card;
    border: 1px solid $border-color;
    border-radius: $radius-lg;
    padding: 32px 28px;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  &__row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 14px;

    @media (max-width: 540px) { grid-template-columns: 1fr; }
  }

  label {
    display: flex;
    flex-direction: column;
    gap: 8px;
    font-size: 0.82rem;
    font-weight: 600;
    color: $text-secondary;
  }

  input, textarea {
    font-family: $font-body;
    font-size: 0.92rem;
    color: $text-primary;
    background: $bg-primary;
    border: 1px solid $border-color;
    border-radius: 8px;
    padding: 12px 14px;
    outline: none;
    resize: vertical;

    &::placeholder { color: $text-muted; }

    &:focus {
      border-color: $accent;
      box-shadow: 0 0 0 3px rgba($accent, 0.12);
    }
  }

  .btn-primary {
    justify-content: center;
    width: 100%;

    &:disabled {
      opacity: 0.75;
      cursor: not-allowed;
      transform: none;
    }
  }
}

.spinner {
  width: 14px;
  height: 14px;
  border: 2px solid rgba(6, 19, 17, 0.25);
  border-top-color: $ink;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin { to { transform: rotate(360deg); } }
</style>
