<script setup lang="ts">
import { computed } from 'vue'
import { useData, withBase } from 'vitepress'
import { pathIn, saveChoice, type Language } from './languages'

const { page, localeIndex } = useData()

const other = computed<Language>(() => (localeIndex.value === 'en' ? 'es' : 'en'))
const label = computed(() => (other.value === 'es' ? 'Español' : 'English'))
const target = computed(() => withBase(pathIn(other.value, page.value.relativePath)))
</script>

<template>
  <a class="language-switch" :href="target" :hreflang="other" :lang="other" @click="saveChoice(other)">
    <svg viewBox="0 0 24 24" width="16" height="16" aria-hidden="true">
      <circle cx="12" cy="12" r="9" />
      <path d="M3 12h18M12 3c2.6 2.6 3.8 5.6 3.8 9s-1.2 6.4-3.8 9c-2.6-2.6-3.8-5.6-3.8-9S9.4 5.6 12 3Z" />
    </svg>
    {{ label }}
  </a>
</template>

<style scoped>
.language-switch {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  margin-left: 12px;
  min-height: 36px;
  padding: 0 14px;
  border: 1.5px solid var(--vp-c-divider);
  border-radius: 999px;
  background: var(--tq-paper);
  color: var(--vp-c-brand-1);
  font-size: 14px;
  font-weight: 500;
  white-space: nowrap;
  transition: background-color 0.2s, color 0.2s, border-color 0.2s;
}

.language-switch:hover {
  background-color: var(--vp-c-brand-1);
  border-color: var(--vp-c-brand-1);
  color: var(--vp-button-brand-text);
}

.language-switch:focus-visible {
  outline: 3px solid var(--vp-c-brand-1);
  outline-offset: 2px;
}

.language-switch svg {
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
}
</style>
