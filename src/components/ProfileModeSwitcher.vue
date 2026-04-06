<script setup lang="ts">
type ProfileMode = 'dev' | 'human'

const props = defineProps<{
  activeMode: ProfileMode
  modeOptions: ReadonlyArray<{
    id: ProfileMode
    label: string
  }>
  revealDelayMs: number
}>()

const revealDelayDuration = `${props.revealDelayMs}ms`

const emit = defineEmits<{
  'update:activeMode': [mode: ProfileMode]
}>()
</script>

<template>
  <div class="mode-switcher" role="group" aria-label="Language mode">
    <span class="mode-switcher-bracket" aria-hidden="true">[</span>
    <span class="mode-switcher-label">lang:</span>
    <button
      v-for="(mode, index) in modeOptions"
      :key="mode.id"
      type="button"
      :class="['mode-button', { active: activeMode === mode.id }]"
      :aria-pressed="activeMode === mode.id"
      @click="emit('update:activeMode', mode.id)"
    >
      <span class="mode-button-label">{{ mode.label }}</span>
      <span
        v-if="index < modeOptions.length - 1"
        class="mode-divider"
        aria-hidden="true"
      >
        |
      </span>
    </button>
    <span class="mode-switcher-bracket" aria-hidden="true">]</span>
  </div>
</template>

<style scoped>
.mode-switcher {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.75rem;
  color: var(--text-muted);
  animation: reveal-mode-switch 420ms ease v-bind(revealDelayDuration) both;
}

.mode-switcher-label {
  color: var(--text-muted);
}

.mode-switcher-bracket {
  color: var(--panel-border-strong);
}

.mode-button {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  border: 0;
  padding: 0;
  background: transparent;
  color: var(--text-secondary);
  cursor: pointer;
  transition: color 140ms ease;
}

.mode-button:hover,
.mode-button:focus-visible,
.mode-button.active {
  color: var(--text-primary);
}

.mode-button-label {
  transition: text-decoration-color 140ms ease;
  text-decoration: underline;
  text-decoration-color: transparent;
  text-underline-offset: 0.22em;
}

.mode-button:hover .mode-button-label,
.mode-button:focus-visible .mode-button-label,
.mode-button.active .mode-button-label {
  text-decoration-color: currentColor;
}

.mode-divider {
  color: var(--text-muted);
}

@keyframes reveal-mode-switch {
  from {
    opacity: 0;
    transform: translateY(0.3rem);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .mode-switcher {
    animation: none;
  }
}
</style>
