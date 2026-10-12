<script setup lang="ts">
import { computed } from 'vue'
import { useData, withBase } from 'vitepress'

type State = 'contact' | 'identified' | 'spam' | 'unknown'
type Icon = 'colors' | 'lock' | 'name' | 'block' | 'share' | 'code'

interface Feature {
  icon: Icon
  state: State
  title: string
  details: string
  link: string
}

interface Call {
  state: State
  name: string
  details: string
}

const repo = 'https://github.com/fsoftt/Tranqui'

const texts = {
  es: {
    titleLead: 'Contesta',
    titleMark: 'tranquilo',
    tagline:
      'Identifica quién llama y bloquea el spam sin exponer tus contactos. Código abierto, pensado para la privacidad y hecho en Colombia.',
    actions: [
      { text: 'Cómo funciona', link: '/guia/como-funciona', brand: true },
      { text: 'Cómo protegemos tus datos', link: '/privacidad/', brand: false },
      { text: 'Ver el código', link: repo, brand: false },
    ],
    incoming: 'Llamada entrante',
    privacy: {
      title: 'Cómo protegemos tus datos',
      key: 'clave secreta',
      lead: 'Nunca guardamos números de teléfono.',
      details:
        'Solo un código (hash) calculado con una clave secreta. Si alguien robara la base de datos, no obtendría números.',
      link: '/privacidad/',
      more: 'Leer más',
    },
    features: [
      {
        icon: 'colors',
        state: 'identified',
        title: 'Un aviso de color en cada llamada',
        details:
          'Verde si está en tus contactos, azul si la comunidad lo identifica, rojo si es spam y naranja si no hay datos. Siempre con ícono y texto, no solo color.',
        link: '/guia/como-funciona',
      },
      {
        icon: 'lock',
        state: 'contact',
        title: 'Nunca guardamos números de teléfono',
        details:
          'Solo un código (hash) calculado con una clave secreta. Si alguien robara la base de datos, no obtendría números.',
        link: '/privacidad/',
      },
      {
        icon: 'name',
        state: 'identified',
        title: 'Nombres protegidos',
        details:
          'Los nombres se guardan cifrados y solo se muestran si varias personas distintas los usaron. Nombres como "Mamá" o "Amor" nunca se usan.',
        link: '/privacidad/',
      },
      {
        icon: 'block',
        state: 'spam',
        title: 'Tú decides qué bloquear',
        details:
          'Bloquea números, el spam reportado por la comunidad, los números privados o las llamadas internacionales. Te avisamos cada vez que bloqueamos una llamada.',
        link: '/guia/como-funciona',
      },
      {
        icon: 'share',
        state: 'unknown',
        title: 'Aportar es opcional',
        details:
          'Puedes aportar tu agenda para ayudar a otros, o no hacerlo. La app funciona igual, y puedes retirar tu aporte cuando quieras.',
        link: '/privacidad/#aportar-tu-agenda',
      },
      {
        icon: 'code',
        state: 'contact',
        title: 'Código abierto',
        details:
          'Todo el código es público bajo la licencia AGPL-3.0, así que cualquiera puede revisar que hacemos lo que decimos.',
        link: '/proyecto/arquitectura',
      },
    ] as Feature[],
    callsTitle: 'Así se ve una llamada',
    calls: [
      { state: 'contact', name: 'Mamá', details: 'Está en tus contactos.' },
      { state: 'identified', name: 'Pizzería Juan', details: 'Así lo conoce la comunidad.' },
      { state: 'spam', name: 'Spam Claro', details: 'Reportado por 23 personas.' },
      { state: 'unknown', name: '+57 •••••••567', details: 'No tenemos datos de este número.' },
    ] as Call[],
    note: 'Tranqui está en desarrollo. La primera versión será para Android y se lanzará primero en Colombia.',
  },
  en: {
    titleLead: 'Answer',
    titleMark: 'calmly',
    tagline:
      'Know who is calling and block spam without exposing your contacts. Open source, privacy first, made in Colombia.',
    actions: [
      { text: 'How it works', link: '/en/guia/como-funciona', brand: true },
      { text: 'How we protect your data', link: '/en/privacidad/', brand: false },
      { text: 'View the code', link: repo, brand: false },
    ],
    incoming: 'Incoming call',
    privacy: {
      title: 'How we protect your data',
      key: 'secret key',
      lead: 'We never store phone numbers.',
      details:
        'Only a code (hash) computed with a secret key. Someone who stole the database would not get any numbers.',
      link: '/en/privacidad/',
      more: 'Read more',
    },
    features: [
      {
        icon: 'colors',
        state: 'identified',
        title: 'A colored card on every call',
        details:
          'Green if the caller is in your contacts, blue if the community knows them, red if it is spam, orange if there is no data. Always with an icon and text, never color alone.',
        link: '/en/guia/como-funciona',
      },
      {
        icon: 'lock',
        state: 'contact',
        title: 'We never store phone numbers',
        details:
          'Only a code (hash) computed with a secret key. Someone who stole the database would not get any numbers.',
        link: '/en/privacidad/',
      },
      {
        icon: 'name',
        state: 'identified',
        title: 'Protected names',
        details:
          'Names are stored encrypted and only shown when several different people used them. Names like "Mom" or "Love" are never used.',
        link: '/en/privacidad/',
      },
      {
        icon: 'block',
        state: 'spam',
        title: 'You decide what to block',
        details:
          'Block numbers, spam reported by the community, private numbers or international calls. We notify you every time we block a call.',
        link: '/en/guia/como-funciona',
      },
      {
        icon: 'share',
        state: 'unknown',
        title: 'Contributing is optional',
        details:
          'You can contribute your address book to help others, or not. The app works the same, and you can withdraw your contribution any time.',
        link: '/en/privacidad/#contributing-your-address-book',
      },
      {
        icon: 'code',
        state: 'contact',
        title: 'Open source',
        details: 'All the code is public under the AGPL-3.0 license, so anyone can check that we do what we say.',
        link: '/en/proyecto/arquitectura',
      },
    ] as Feature[],
    callsTitle: 'What a call looks like',
    calls: [
      { state: 'contact', name: 'Mom', details: 'In your contacts.' },
      { state: 'identified', name: 'Pizzería Juan', details: 'How the community knows them.' },
      { state: 'spam', name: 'Spam Claro', details: 'Reported by 23 people.' },
      { state: 'unknown', name: '+57 •••••••567', details: 'We have no data about this number.' },
    ] as Call[],
    note: 'Tranqui is in development. The first version will be for Android and will launch in Colombia first.',
  },
}

const { localeIndex } = useData()
const t = computed(() => (localeIndex.value === 'en' ? texts.en : texts.es))

const isExternal = (link: string) => /^https?:\/\//.test(link)
const href = (link: string) => (isExternal(link) ? link : withBase(link))

/** Simple 24px line icons, drawn with the current text color. */
const icons: Record<Icon, string> = {
  colors:
    '<rect x="3" y="4" width="18" height="16" rx="4"/><path d="M7 9h6M7 13h10"/><circle cx="17" cy="9" r="1.2" fill="currentColor"/>',
  lock: '<rect x="5" y="10.5" width="14" height="10" rx="3"/><path d="M8 10.5V8a4 4 0 0 1 8 0v2.5M12 14.5v2.5"/>',
  name: '<path d="M12 3.5 19 6v5.5c0 4.3-2.9 7.6-7 9-4.1-1.4-7-4.7-7-9V6z"/><circle cx="12" cy="10" r="2.2"/><path d="M8.6 15.6c.8-1.5 2-2.2 3.4-2.2s2.6.7 3.4 2.2"/>',
  block: '<circle cx="12" cy="12" r="8.5"/><path d="m6 6 12 12"/>',
  share:
    '<circle cx="8" cy="9" r="3"/><path d="M3 19c.6-3 2.6-4.7 5-4.7s4.4 1.7 5 4.7"/><path d="M17 7v6M14 10h6"/>',
  code: '<path d="m8.5 8-4 4 4 4M15.5 8l4 4-4 4M13.5 5.5l-3 13"/>',
}

const stateIcons: Record<State, string> = {
  contact: '<path d="m7 12.5 3.2 3.2L17 9"/>',
  identified: '<path d="M12 11v5.5"/><circle cx="12" cy="7.8" r="1.1" fill="currentColor" stroke="none"/>',
  spam: '<path d="M12 7.5v5.5"/><circle cx="12" cy="16.4" r="1.1" fill="currentColor" stroke="none"/>',
  unknown:
    '<path d="M9.6 9.4a2.5 2.5 0 1 1 3.6 2.3c-.8.4-1.2.9-1.2 1.8v.4"/><circle cx="12" cy="16.9" r="1.1" fill="currentColor" stroke="none"/>',
}
</script>

<template>
  <div class="th">
    <!-- Hero -->
    <section class="th-hero">
      <div class="th-hero-copy">
        <p class="th-brand">
          <img :src="withBase('/favicon.svg')" alt="" width="36" height="36" />
          <span>Tranqui</span>
        </p>
        <h1 class="th-title">
          {{ t.titleLead }}
          <span class="th-mark">
            {{ t.titleMark }}
            <svg class="th-wave" viewBox="0 0 300 24" preserveAspectRatio="none" aria-hidden="true">
              <path
                d="M4 15c18-9 34-9 50-1s32 9 49 0 34-10 51-1 33 9 50 0 34-9 50-1 25 6 42 1"
                pathLength="1"
              />
            </svg>
          </span>
        </h1>
        <p class="th-tagline">{{ t.tagline }}</p>
        <div class="th-actions">
          <a
            v-for="action in t.actions"
            :key="action.link"
            class="th-button"
            :class="action.brand ? 'is-brand' : 'is-alt'"
            :href="href(action.link)"
            :target="isExternal(action.link) ? '_blank' : undefined"
            :rel="isExternal(action.link) ? 'noreferrer' : undefined"
          >
            <svg v-if="isExternal(action.link)" class="th-gh" viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M12 2a10 10 0 0 0-3.16 19.49c.5.09.68-.22.68-.48v-1.7c-2.78.6-3.37-1.34-3.37-1.34-.45-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.9 1.52 2.34 1.08 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.94 0-1.09.39-1.98 1.03-2.68-.1-.25-.45-1.27.1-2.65 0 0 .84-.27 2.75 1.02a9.6 9.6 0 0 1 5 0c1.91-1.29 2.75-1.02 2.75-1.02.55 1.38.2 2.4.1 2.65.64.7 1.03 1.59 1.03 2.68 0 3.84-2.34 4.68-4.57 4.93.36.31.68.92.68 1.85v2.75c0 .27.18.58.69.48A10 10 0 0 0 12 2Z"
              />
            </svg>
            {{ action.text }}
            <span v-if="action.brand" class="th-arrow" aria-hidden="true">→</span>
          </a>
        </div>
      </div>

      <!-- Decorative: the same four calls are described in text further down the page. -->
      <div class="th-hero-art" aria-hidden="true">
        <div class="th-waves">
          <span v-for="ring in 4" :key="ring" :style="{ '--ring': ring }" />
        </div>
        <div class="th-phone">
          <div class="th-screen">
            <div class="th-status"><span>9:41</span><span class="th-notch" /><span>5G</span></div>
            <p class="th-incoming">{{ t.incoming }}</p>
            <div class="th-call-stack">
              <div v-for="(call, index) in t.calls" :key="call.state" class="th-call" :class="`is-${call.state}`" :style="{ '--i': index }">
                <span class="th-avatar">
                  <svg viewBox="0 0 24 24" v-html="stateIcons[call.state]" />
                </span>
                <strong class="th-call-name">{{ call.name }}</strong>
                <span class="th-call-details">{{ call.details }}</span>
              </div>
            </div>
            <div class="th-progress">
              <span v-for="(call, index) in t.calls" :key="call.state" :class="`is-${call.state}`" :style="{ '--i': index }" />
            </div>
            <div class="th-call-buttons">
              <span class="th-decline">
                <svg viewBox="0 0 24 24"><path d="M20.5 16.4v2.8a1.9 1.9 0 0 1-2 1.9A18.6 18.6 0 0 1 2.9 5.5a1.9 1.9 0 0 1 1.9-2h2.8a1.9 1.9 0 0 1 1.9 1.6c.1.9.4 1.8.7 2.6a1.9 1.9 0 0 1-.4 2l-1.2 1.2a15 15 0 0 0 5.6 5.6l1.2-1.2a1.9 1.9 0 0 1 2-.4c.8.3 1.7.6 2.6.7a1.9 1.9 0 0 1 1.6 1.9z" /></svg>
              </span>
              <span class="th-answer">
                <svg viewBox="0 0 24 24"><path d="M20.5 16.4v2.8a1.9 1.9 0 0 1-2 1.9A18.6 18.6 0 0 1 2.9 5.5a1.9 1.9 0 0 1 1.9-2h2.8a1.9 1.9 0 0 1 1.9 1.6c.1.9.4 1.8.7 2.6a1.9 1.9 0 0 1-.4 2l-1.2 1.2a15 15 0 0 0 5.6 5.6l1.2-1.2a1.9 1.9 0 0 1 2-.4c.8.3 1.7.6 2.6.7a1.9 1.9 0 0 1 1.6 1.9z" /></svg>
              </span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Privacy explainer -->
    <section class="th-section th-privacy">
      <div class="th-privacy-card">
        <h2 class="th-h2">{{ t.privacy.title }}</h2>
        <div class="th-flow" aria-hidden="true">
          <span class="th-chip th-chip-number">
            <svg viewBox="0 0 24 24"><rect x="7" y="2.5" width="10" height="19" rx="2.5" /><path d="M11 18.5h2" /></svg>
            +57 300 •••• 567
          </span>
          <span class="th-flow-arrow">
            <span class="th-flow-key">
              <svg viewBox="0 0 24 24"><circle cx="8" cy="12" r="3.5" /><path d="M11.5 12H21M17.5 12v3M20.5 12v2" /></svg>
              {{ t.privacy.key }}
            </span>
            <svg class="th-flow-line" viewBox="0 0 120 16" preserveAspectRatio="none">
              <path d="M2 8c20-6 40 6 60 0s36-6 52 0" />
              <path d="m108 3 6 5-6 5" />
            </svg>
          </span>
          <span class="th-chip th-chip-hash">
            <svg viewBox="0 0 24 24"><path d="M9 4 7 20M17 4l-2 16M4.5 9h15M4 15h15" /></svg>
            9f2c 41ab e07d …
          </span>
        </div>
        <p class="th-privacy-text">
          <strong>{{ t.privacy.lead }}</strong> {{ t.privacy.details }}
          <a :href="href(t.privacy.link)" class="th-link">{{ t.privacy.more }}<span aria-hidden="true"> →</span></a>
        </p>
      </div>
    </section>

    <!-- Features -->
    <section class="th-section">
      <ul class="th-features">
        <li v-for="feature in t.features" :key="feature.title">
          <a class="th-feature" :href="href(feature.link)">
            <span class="th-feature-icon">
              <svg viewBox="0 0 24 24" aria-hidden="true" v-html="icons[feature.icon]" />
              <span class="th-dot" :class="`is-${feature.state}`" aria-hidden="true" />
            </span>
            <h2 class="th-feature-title">{{ feature.title }}</h2>
            <p class="th-feature-details">{{ feature.details }}</p>
          </a>
        </li>
      </ul>
    </section>

    <!-- What a call looks like -->
    <section class="th-section">
      <h2 class="th-h2 th-center">{{ t.callsTitle }}</h2>
      <ul class="th-states">
        <li v-for="call in t.calls" :key="call.state" class="th-state" :class="`is-${call.state}`">
          <span class="th-state-icon">
            <svg viewBox="0 0 24 24" aria-hidden="true" v-html="stateIcons[call.state]" />
          </span>
          <span class="th-state-text">
            <strong>{{ call.name }}</strong>
            <span>{{ call.details }}</span>
          </span>
        </li>
      </ul>

      <p class="th-note">
        <svg viewBox="0 0 24 24" aria-hidden="true"><path d="M3 12c2.5-3 5.5-3 9 0s6.5 3 9 0M3 17c2.5-3 5.5-3 9 0s6.5 3 9 0M3 7c2.5-3 5.5-3 9 0s6.5 3 9 0" /></svg>
        <span>{{ t.note }}</span>
      </p>
    </section>
  </div>
</template>

<style scoped>
.th {
  --th-radius: 24px;
  max-width: 1152px;
  margin: 0 auto;
  padding: 0 24px;
  color: var(--vp-c-text-1);
}

svg {
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* ---------- Hero ---------- */
.th-hero {
  display: grid;
  grid-template-columns: minmax(0, 1.25fr) minmax(0, 0.75fr);
  align-items: center;
  gap: 48px;
  padding: 72px 0 56px;
}

.th-brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 20px;
  padding: 6px 16px 6px 6px;
  border: 1px solid var(--vp-c-divider);
  border-radius: 999px;
  background: var(--tq-paper);
  font-family: var(--tq-font-display);
  font-size: 18px;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.th-brand img {
  border-radius: 10px;
}

.th-title {
  margin: 0;
  font-family: var(--tq-font-display);
  font-variation-settings: 'SOFT' 100, 'opsz' 144;
  font-weight: 600;
  font-size: clamp(48px, 7.2vw, 88px);
  line-height: 1.02;
  letter-spacing: -0.025em;
  color: var(--vp-c-text-1);
}

.th-mark {
  position: relative;
  display: inline-block;
  font-style: italic;
  font-weight: 500;
  color: var(--vp-c-brand-1);
  white-space: nowrap;
}

.th-wave {
  position: absolute;
  left: 2%;
  bottom: -0.16em;
  width: 96%;
  height: 0.26em;
  overflow: visible;
}

.th-wave path {
  stroke: var(--tq-sand-strong);
  stroke-width: 7;
  stroke-dasharray: 1;
  stroke-dashoffset: 0;
  animation: th-draw 1.6s 0.3s cubic-bezier(0.65, 0, 0.35, 1) both;
}

@keyframes th-draw {
  from { stroke-dashoffset: 1; }
}

.th-tagline {
  max-width: 34em;
  margin: 32px 0 0;
  font-size: 19px;
  line-height: 1.6;
  color: var(--vp-c-text-2);
}

.th-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 32px;
}

.th-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  min-height: 48px;
  padding: 0 22px;
  border-radius: 999px;
  border: 1.5px solid transparent;
  font-size: 15.5px;
  font-weight: 600;
  text-decoration: none;
  transition: background-color 0.25s, border-color 0.25s, transform 0.25s, box-shadow 0.25s;
}

.th-button.is-brand {
  background: var(--vp-button-brand-bg);
  color: var(--vp-button-brand-text);
  box-shadow: 0 10px 24px -12px var(--tq-shadow-brand);
}

.th-button.is-brand:hover {
  background: var(--vp-button-brand-hover-bg);
  transform: translateY(-1px);
}

.th-button.is-alt {
  background: var(--tq-paper);
  border-color: var(--vp-c-divider);
  color: var(--vp-c-text-1);
}

.th-button.is-alt:hover {
  border-color: var(--vp-c-brand-1);
  color: var(--vp-c-brand-1);
}

.th-button:focus-visible,
.th-feature:focus-visible,
.th-link:focus-visible {
  outline: 3px solid var(--vp-c-brand-1);
  outline-offset: 3px;
}

.th-gh {
  width: 18px;
  height: 18px;
  fill: currentColor;
  stroke: none;
}

.th-arrow {
  transition: transform 0.25s;
}

.th-button:hover .th-arrow {
  transform: translateX(3px);
}

/* Phone mock */
.th-hero-art {
  position: relative;
  display: grid;
  place-items: center;
  min-height: 560px;
}

.th-waves {
  position: absolute;
  inset: 0;
  display: grid;
  place-items: center;
}

.th-waves span {
  position: absolute;
  width: calc(230px + var(--ring) * 92px);
  aspect-ratio: 1;
  border-radius: 50%;
  border: 1.5px solid var(--tq-wave);
  background: radial-gradient(circle, transparent 60%, var(--tq-wave-fill));
  opacity: calc(1 - var(--ring) * 0.2);
  animation: th-breathe 7s ease-in-out infinite;
  animation-delay: calc(var(--ring) * -0.9s);
}

@keyframes th-breathe {
  0%, 100% { transform: scale(0.97); }
  50% { transform: scale(1.03); }
}

.th-phone {
  position: relative;
  width: 264px;
  padding: 10px;
  border-radius: 46px;
  background: #10233a;
  box-shadow:
    0 0 0 1.5px #2a4560 inset,
    0 40px 70px -30px rgba(16, 35, 58, 0.55),
    0 18px 30px -18px rgba(16, 35, 58, 0.4);
  transform: rotate(-3deg);
}

.th-screen {
  position: relative;
  display: flex;
  flex-direction: column;
  height: 520px;
  padding: 14px 18px 26px;
  border-radius: 37px;
  background:
    radial-gradient(120% 70% at 50% 0%, #1d5fb8 0%, transparent 70%),
    linear-gradient(180deg, #0f4c81 0%, #0b3558 100%);
  color: #ffffff;
  overflow: hidden;
}

.th-status {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 12px;
  font-weight: 600;
  padding: 0 6px;
}

.th-notch {
  width: 74px;
  height: 22px;
  border-radius: 999px;
  background: #10233a;
}

.th-incoming {
  margin: 36px 0 18px;
  text-align: center;
  font-size: 13px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #dce9f5;
}

.th-call-stack {
  position: relative;
  height: 236px;
}

.th-call {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 26px 16px 20px;
  border-radius: 26px;
  background: #fffcf6;
  color: #10233a;
  border-top: 6px solid var(--state);
  box-shadow: 0 20px 40px -20px rgba(0, 0, 0, 0.5);
  opacity: 0;
  animation: th-cycle 16s infinite;
  animation-delay: calc(var(--i) * 4s - 0.8s);
}

@keyframes th-cycle {
  0% { opacity: 0; transform: translateY(10px) scale(0.98); }
  5%, 22% { opacity: 1; transform: none; }
  27%, 100% { opacity: 0; transform: translateY(-6px) scale(0.99); }
}

.th-avatar {
  display: grid;
  place-items: center;
  width: 72px;
  height: 72px;
  margin-bottom: 14px;
  border-radius: 50%;
  background: var(--state);
  color: #ffffff;
  box-shadow: 0 0 0 8px var(--state-soft);
}

.th-avatar svg {
  width: 36px;
  height: 36px;
  stroke-width: 2.4;
}

.th-call-name {
  font-family: var(--tq-font-display);
  font-size: 23px;
  font-weight: 600;
  line-height: 1.2;
}

.th-call-details {
  margin-top: 6px;
  font-size: 14px;
  line-height: 1.4;
  font-weight: 600;
  color: var(--state);
}

.th-progress {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-top: 18px;
}

.th-progress span {
  width: 22px;
  height: 5px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.25);
  position: relative;
  overflow: hidden;
}

.th-progress span::after {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--state-on-dark);
  opacity: 0;
  animation: th-cycle-dot 16s infinite;
  animation-delay: calc(var(--i) * 4s - 0.8s);
}

@keyframes th-cycle-dot {
  0% { opacity: 0; }
  5%, 22% { opacity: 1; }
  27%, 100% { opacity: 0; }
}

.th-call-buttons {
  display: flex;
  justify-content: space-between;
  margin-top: auto;
  padding: 0 18px;
}

.th-call-buttons > span {
  display: grid;
  place-items: center;
  width: 56px;
  height: 56px;
  border-radius: 50%;
}

.th-call-buttons svg {
  width: 26px;
  height: 26px;
  fill: #ffffff;
  stroke: none;
}

.th-decline { background: #c0262d; }
.th-decline svg { transform: rotate(135deg); }
.th-answer svg { transform: rotate(0deg); }
.th-answer { background: #1e7b34; }

/* ---------- State colors (always shown with an icon and text) ---------- */
.is-contact { --state: #1e7b34; --state-soft: #dcefe0; --state-on-dark: #7ed99a; }
.is-identified { --state: #1d5fb8; --state-soft: #dce9f5; --state-on-dark: #9cc6ef; }
.is-spam { --state: #c0262d; --state-soft: #f8dfdf; --state-on-dark: #ff9a9f; }
.is-unknown { --state: #b45309; --state-soft: #f7e6d3; --state-on-dark: #f4b77a; }

/* ---------- Sections ---------- */
.th-section {
  padding: 40px 0;
}

.th-h2 {
  margin: 0 0 24px;
  font-family: var(--tq-font-display);
  font-variation-settings: 'SOFT' 100;
  font-weight: 600;
  font-size: clamp(28px, 4vw, 38px);
  line-height: 1.15;
  letter-spacing: -0.015em;
  color: var(--vp-c-text-1);
}

.th-center {
  text-align: center;
}

/* Privacy explainer */
.th-privacy-card {
  position: relative;
  padding: 44px 48px;
  border-radius: 28px;
  background: var(--tq-paper);
  border: 1px solid var(--vp-c-divider);
  box-shadow: var(--tq-shadow);
  overflow: hidden;
}

.th-privacy-card::before {
  content: '';
  position: absolute;
  inset: 0 0 auto;
  height: 6px;
  background: linear-gradient(90deg, #1e7b34 0 25%, #1d5fb8 25% 50%, #c0262d 50% 75%, #b45309 75%);
}

.th-flow {
  display: flex;
  align-items: center;
  gap: 16px;
  flex-wrap: wrap;
}

.th-chip {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 14px 20px;
  border-radius: 18px;
  font-family: ui-monospace, 'SFMono-Regular', Menlo, Consolas, monospace;
  font-size: 18px;
  font-weight: 600;
  white-space: nowrap;
}

.th-chip svg {
  width: 22px;
  height: 22px;
  flex: none;
}

.th-chip-number {
  background: var(--tq-sand);
  color: var(--vp-c-text-1);
}

.th-chip-hash {
  background: var(--vp-c-brand-soft);
  color: var(--vp-c-brand-1);
  letter-spacing: 0.04em;
}

.th-flow-arrow {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  min-width: 140px;
  flex: 1 1 140px;
  max-width: 240px;
  color: var(--vp-c-text-2);
  font-size: 13px;
  font-weight: 600;
}

.th-flow-key {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.th-flow-key svg {
  width: 18px;
  height: 18px;
}

.th-flow-line {
  width: 100%;
  height: 16px;
  stroke-width: 2;
  color: var(--vp-c-brand-1);
}

.th-privacy-text {
  max-width: 46em;
  margin: 28px 0 0;
  font-size: 17px;
  line-height: 1.7;
  color: var(--vp-c-text-2);
}

.th-privacy-text strong {
  color: var(--vp-c-text-1);
}

.th-link {
  margin-left: 4px;
  font-weight: 600;
  color: var(--vp-c-brand-1);
  text-decoration: underline;
  text-decoration-color: var(--tq-sand-strong);
  text-decoration-thickness: 2px;
  text-underline-offset: 4px;
  white-space: nowrap;
}

/* Features */
.th-features {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 20px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.th-feature {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 28px;
  border-radius: var(--th-radius);
  background: var(--tq-paper);
  border: 1px solid var(--vp-c-divider);
  color: inherit;
  text-decoration: none;
  transition: transform 0.3s, box-shadow 0.3s, border-color 0.3s;
}

.th-feature:hover {
  transform: translateY(-3px);
  border-color: var(--tq-line-strong);
  box-shadow: var(--tq-shadow);
}

.th-feature-icon {
  position: relative;
  display: grid;
  place-items: center;
  width: 52px;
  height: 52px;
  margin-bottom: 20px;
  border-radius: 16px;
  background: var(--vp-c-brand-soft);
  color: var(--vp-c-brand-1);
}

.th-feature-icon svg {
  width: 26px;
  height: 26px;
}

.th-dot {
  position: absolute;
  top: -3px;
  right: -3px;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: var(--state);
  border: 3px solid var(--tq-paper);
}

.th-feature-title {
  margin: 0 0 8px;
  font-family: var(--tq-font-display);
  font-variation-settings: 'SOFT' 100;
  font-size: 21px;
  font-weight: 600;
  line-height: 1.3;
  color: var(--vp-c-text-1);
}

.th-feature-details {
  margin: 0;
  font-size: 15px;
  line-height: 1.65;
  color: var(--vp-c-text-2);
}

/* States */
.th-states {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 16px;
  margin: 0;
  padding: 0;
  list-style: none;
}

.th-state {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 16px;
  padding: 24px 22px;
  border-radius: var(--th-radius);
  background: var(--tq-paper);
  border: 1px solid var(--vp-c-divider);
  border-top: 6px solid var(--state);
  box-shadow: var(--tq-shadow-soft);
}

.th-state-icon {
  display: grid;
  place-items: center;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: var(--state);
  color: #ffffff;
  box-shadow: 0 0 0 6px var(--state-soft);
}

.th-state-icon svg {
  width: 26px;
  height: 26px;
  stroke-width: 2.4;
}

.th-state-text strong {
  display: block;
  margin-bottom: 4px;
  font-family: var(--tq-font-display);
  font-size: 21px;
  font-weight: 600;
  color: var(--vp-c-text-1);
}

.th-state-text span {
  font-size: 15px;
  line-height: 1.5;
  color: var(--vp-c-text-2);
}

.th-note {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  max-width: 720px;
  margin: 48px auto 0;
  padding: 18px 24px;
  border-radius: 999px;
  background: var(--tq-sand);
  color: var(--vp-c-text-1);
  font-size: 15.5px;
  line-height: 1.5;
  text-align: left;
}

.th-note svg {
  flex: none;
  width: 26px;
  height: 26px;
  color: var(--vp-c-brand-1);
}

/* ---------- Dark mode ---------- */
.dark .th-state-icon,
.dark .th-avatar {
  box-shadow: 0 0 0 6px color-mix(in srgb, var(--state) 30%, transparent);
}

.dark .th-call {
  box-shadow: 0 20px 40px -16px rgba(0, 0, 0, 0.7);
}

.dark .th-phone {
  background: #050d17;
  box-shadow:
    0 0 0 1.5px #2a4560 inset,
    0 40px 80px -30px rgba(0, 0, 0, 0.8);
}

/* ---------- Responsive ---------- */
@media (max-width: 959px) {
  .th-hero {
    grid-template-columns: minmax(0, 1fr);
    gap: 24px;
    padding-top: 48px;
  }

  .th-features {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .th-states {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
}

@media (max-width: 639px) {
  .th {
    padding: 0 16px;
  }

  .th-hero {
    padding: 32px 0 24px;
  }

  .th-tagline {
    font-size: 17px;
    margin-top: 26px;
  }

  .th-actions {
    flex-direction: column;
    align-items: stretch;
  }

  .th-button {
    justify-content: center;
  }

  .th-hero-art {
    min-height: 470px;
  }

  .th-waves span {
    width: calc(180px + var(--ring) * 70px);
  }

  .th-phone {
    width: 232px;
  }

  .th-screen {
    height: 450px;
  }

  .th-incoming {
    margin-top: 26px;
  }

  .th-call-stack {
    height: 214px;
  }

  .th-privacy-card {
    padding: 32px 22px;
  }

  .th-flow {
    flex-direction: column;
    align-items: stretch;
    gap: 10px;
  }

  .th-chip {
    justify-content: center;
    font-size: 16px;
  }

  .th-flow-arrow {
    flex: none;
    min-width: 0;
    max-width: none;
  }

  .th-flow-line {
    transform: rotate(90deg);
    width: 44px;
    height: 16px;
    margin: 10px 0;
  }

  .th-features,
  .th-states {
    grid-template-columns: minmax(0, 1fr);
  }

  .th-feature {
    padding: 24px;
  }

  .th-note {
    border-radius: 22px;
  }
}

/* ---------- Reduced motion: no cycling, show the first call ---------- */
@media (prefers-reduced-motion: reduce) {
  .th-call,
  .th-progress span::after,
  .th-waves span,
  .th-wave path {
    animation: none;
  }

  .th-call:first-child,
  .th-progress span:first-child::after {
    opacity: 1;
  }

  .th-button,
  .th-feature,
  .th-arrow {
    transition: none;
  }

  .th-feature:hover,
  .th-button.is-brand:hover {
    transform: none;
  }
}
</style>
