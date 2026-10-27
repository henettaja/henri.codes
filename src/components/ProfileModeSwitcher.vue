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
    <span class="mode-switcher-label">lang:</span>
    <button
      v-for="(mode, index) in modeOptions"
      :key="mode.id"
      type="button"
      :class="[
        'mode-button',
        `mode-button-${mode.id}`,
        { active: activeMode === mode.id },
      ]"
      :aria-pressed="activeMode === mode.id"
      @click="emit('update:activeMode', mode.id)"
    >
      <span class="mode-button-label">
        <span class="mode-button-label-text" :data-text="mode.label">
          {{ mode.label }}
        </span>
      </span>
      <span
        v-if="index < modeOptions.length - 1"
        class="mode-divider"
        aria-hidden="true"
      >
        |
      </span>
    </button>
  </div>
</template>

<style scoped>
.mode-switcher {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 0.75rem;
  width: fit-content;
  max-width: 100%;
  padding: 0.72rem 2.3rem;
  border: 1px solid var(--panel-border-strong);
  border-radius: 1.2rem;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.025) 0%,
    rgba(255, 255, 255, 0.01) 100%
  );
  color: var(--text-muted);
  animation: reveal-mode-switch 420ms ease v-bind(revealDelayDuration) both;
}

.mode-switcher-label {
  color: var(--text-muted);
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
  position: relative;
  display: inline-block;
  transition: text-decoration-color 140ms ease;
  text-decoration: underline;
  text-decoration-color: transparent;
  text-underline-offset: 0.22em;
}

.mode-button-dev .mode-button-label {
  isolation: isolate;
  overflow: hidden;
  padding: 0.18em 0.28em 0.22em;
  margin: -0.18em -0.28em -0.22em;
  border-radius: 0.28em;
}

.mode-button-label-text {
  position: relative;
  z-index: 1;
}

.mode-button-dev .mode-button-label-text::before,
.mode-button-dev .mode-button-label-text::after {
  content: attr(data-text) / '';
  position: absolute;
  inset: 0;
  z-index: -1;
  opacity: 0;
  pointer-events: none;
  mix-blend-mode: screen;
}

.mode-button-dev .mode-button-label-text::before {
  color: rgba(114, 255, 214, 1);
}

.mode-button-dev .mode-button-label-text::after {
  color: rgba(255, 108, 169, 0.96);
}

.mode-button-dev .mode-button-label::after {
  content: '';
  position: absolute;
  left: -18%;
  right: -18%;
  top: 50%;
  height: 0.34em;
  opacity: 0;
  pointer-events: none;
  background: linear-gradient(
    90deg,
    transparent 0%,
    rgba(114, 255, 214, 0.18) 18%,
    rgba(255, 108, 169, 0.52) 52%,
    rgba(114, 255, 214, 0.18) 82%,
    transparent 100%
  );
  transform: translateX(-130%);
  filter: blur(0.03em);
}

.mode-button:hover .mode-button-label,
.mode-button:focus-visible .mode-button-label,
.mode-button.active .mode-button-label {
  text-decoration-color: currentColor;
}

.mode-button-dev:hover .mode-button-label-text,
.mode-button-dev:focus-visible .mode-button-label-text {
  animation: dev-glitch-base 2311ms step-end -280ms infinite;
  text-shadow:
    -0.12em 0 0 rgba(114, 255, 214, 0.5),
    0.12em 0 0 rgba(255, 108, 169, 0.42);
}

.mode-button-dev:hover .mode-button-label-text::before,
.mode-button-dev:focus-visible .mode-button-label-text::before {
  animation: dev-glitch-left 967ms steps(2, end) -610ms infinite;
}

.mode-button-dev:hover .mode-button-label-text::after,
.mode-button-dev:focus-visible .mode-button-label-text::after {
  animation: dev-glitch-right 787ms steps(2, end) -350ms infinite;
}

.mode-button-dev:hover .mode-button-label::after,
.mode-button-dev:focus-visible .mode-button-label::after {
  animation: dev-scanline 1597ms linear -900ms infinite;
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

@keyframes dev-glitch-base {
  0%,
  100% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }

  7% {
    text-shadow:
      -0.13em 0 0 rgba(114, 255, 214, 1),
      0.13em 0 0 rgba(255, 108, 169, 0.95);
  }

  10% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }

  23% {
    text-shadow:
      -0.09em 0 0 rgba(114, 255, 214, 0.7),
      0.09em 0 0 rgba(255, 108, 169, 0.6);
  }

  25% {
    text-shadow:
      -0.16em 0 0 rgba(114, 255, 214, 1),
      0.05em 0 0 rgba(255, 108, 169, 0.5);
  }

  29% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }

  41% {
    text-shadow:
      -0.04em 0 0 rgba(114, 255, 214, 0.3),
      0.04em 0 0 rgba(255, 108, 169, 0.26);
  }

  52% {
    text-shadow:
      -0.11em 0 0 rgba(114, 255, 214, 0.85),
      0.11em 0 0 rgba(255, 108, 169, 0.8);
  }

  55% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }

  63% {
    text-shadow:
      -0.03em 0 0 rgba(114, 255, 214, 0.3),
      0.14em 0 0 rgba(255, 108, 169, 0.95);
  }

  66% {
    text-shadow:
      -0.08em 0 0 rgba(114, 255, 214, 0.6),
      0.08em 0 0 rgba(255, 108, 169, 0.55);
  }

  71% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }

  86% {
    text-shadow:
      -0.15em 0 0 rgba(114, 255, 214, 1),
      0.15em 0 0 rgba(255, 108, 169, 1);
  }

  88% {
    text-shadow:
      -0.05em 0 0 rgba(114, 255, 214, 0.4),
      0.05em 0 0 rgba(255, 108, 169, 0.34);
  }

  93% {
    text-shadow:
      -0.1em 0 0 rgba(114, 255, 214, 0.75),
      0.1em 0 0 rgba(255, 108, 169, 0.7);
  }

  96% {
    text-shadow:
      -0.06em 0 0 rgba(114, 255, 214, 0.4),
      0.06em 0 0 rgba(255, 108, 169, 0.34);
  }
}

@keyframes dev-glitch-left {
  0%,
  100% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(0 0 100% 0);
  }

  14% {
    opacity: 0.88;
    transform: translate(-0.22em, -0.03em);
    clip-path: inset(8% 0 58% 0);
  }

  24% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(0 0 100% 0);
  }

  46% {
    opacity: 0.78;
    transform: translate(-0.24em, 0.04em);
    clip-path: inset(54% 0 16% 0);
  }

  58% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(0 0 100% 0);
  }

  80% {
    opacity: 0.7;
    transform: translate(-0.18em, 0);
    clip-path: inset(28% 0 38% 0);
  }

  88% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(0 0 100% 0);
  }
}

@keyframes dev-glitch-right {
  0%,
  100% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(100% 0 0 0);
  }

  10% {
    opacity: 0.72;
    transform: translate(0.2em, 0);
    clip-path: inset(24% 0 44% 0);
  }

  20% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(100% 0 0 0);
  }

  38% {
    opacity: 0.68;
    transform: translate(0.24em, -0.04em);
    clip-path: inset(70% 0 8% 0);
  }

  50% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(100% 0 0 0);
  }

  74% {
    opacity: 0.62;
    transform: translate(0.18em, 0.03em);
    clip-path: inset(16% 0 62% 0);
  }

  84% {
    opacity: 0;
    transform: translate(0, 0);
    clip-path: inset(100% 0 0 0);
  }
}

@keyframes dev-scanline {
  0% {
    opacity: 0;
    transform: translateX(-130%);
  }

  6.5% {
    opacity: 1;
  }

  15% {
    opacity: 0.68;
    transform: translateX(24%);
  }

  19.4% {
    opacity: 0;
    transform: translateX(46%);
  }

  100% {
    opacity: 0;
    transform: translateX(140%);
  }
}

@media (prefers-reduced-motion: reduce) {
  .mode-switcher {
    animation: none;
  }

  .mode-button-dev:hover .mode-button-label-text,
  .mode-button-dev:focus-visible .mode-button-label-text,
  .mode-button-dev:hover .mode-button-label-text::before,
  .mode-button-dev:focus-visible .mode-button-label-text::before,
  .mode-button-dev:hover .mode-button-label-text::after,
  .mode-button-dev:focus-visible .mode-button-label-text::after,
  .mode-button-dev:hover .mode-button-label::after,
  .mode-button-dev:focus-visible .mode-button-label::after {
    animation: none;
  }
}

@media (hover: none) {
  .mode-button-dev:hover:not(:focus-visible) .mode-button-label-text,
  .mode-button-dev:hover:not(:focus-visible) .mode-button-label-text::before,
  .mode-button-dev:hover:not(:focus-visible) .mode-button-label-text::after,
  .mode-button-dev:hover:not(:focus-visible) .mode-button-label::after {
    animation: none;
    text-shadow: none;
  }
}
</style>
