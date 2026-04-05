<script setup lang="ts">
import TreeList from './TreeList.vue'
import TreeListItem from './TreeListItem.vue'

defineProps<{
  links: ReadonlyArray<{
    href: string
    label: string
    note: string
    kind: string
  }>
}>()
</script>

<template>
  <aside class="content-section links-panel" aria-labelledby="links-title">
    <h2 id="links-title">./elsewhere</h2>
    <div class="link-tree" aria-label="Links">
      <TreeList>
        <TreeListItem
          v-for="(link, index) in links"
          :key="link.href"
          :is-last="index === links.length - 1"
        >
          <a
            :href="link.href"
            target="_blank"
            rel="noreferrer"
            :class="['link-item', `link-item-${link.kind}`]"
          >
            <span class="link-label">
              <span class="link-label-text">{{ link.label }}</span>
              <span class="link-arrow" aria-hidden="true">↗</span>
            </span>
            <span class="link-note">
              <span
                :class="[
                  'link-note-field',
                  link.kind === 'instagram' ? 'link-note-field-instagram' : '',
                ]"
              >
                <span class="link-note-text">{{ link.note }}</span>
                <span
                  v-if="link.kind === 'instagram'"
                  class="instagram-sparkles"
                  aria-hidden="true"
                >
                  <span class="instagram-sparkle instagram-sparkle-1">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-2">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-3">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-4">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-5">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-6">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-7">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-8">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-9">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-10">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-11">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-12">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-13">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-14">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-15">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-16">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-17">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-18">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-19">✦</span>
                  <span class="instagram-sparkle instagram-sparkle-20">✦</span>
                </span>
              </span>
            </span>
          </a>
        </TreeListItem>
      </TreeList>
    </div>
  </aside>
</template>

<style scoped>
.content-section {
  display: grid;
  gap: 1.5rem;
  padding: 0;
}

#links-title {
  font-size: 2rem;
  font-weight: 500;
}

.link-tree {
  margin-top: 1rem;
}
.link-item {
  display: inline-flex;
  align-items: center;
  flex-wrap: wrap;
  min-height: 0;
  padding: 0;
  transition: color 140ms ease;
  color: var(--text-secondary);
  gap: 0.75rem;
  min-width: 0;
}

.link-item:hover,
.link-item:focus-visible {
  color: var(--text-primary);
}

.link-label {
  display: inline-flex;
  align-items: baseline;
  gap: 0.35rem;
}

.link-label-text {
  text-decoration: underline;
  text-decoration-color: currentColor;
  text-underline-offset: 0.25em;
}

.link-arrow {
  font-size: 0.9em;
  opacity: 0.72;
  transform: translateY(0.055em);
}

.link-note {
  display: inline-block;
  color: var(--text-muted);
  font-size: 1.5rem;
}

.link-note-field {
  position: relative;
  display: inline-block;
}

.link-note-field-instagram {
  padding: 0.9em 1.3em;
  margin: -0.9em -1.3em;
}

.link-note-text {
  display: inline-block;
}

.link-item-personal .link-label {
  color: var(--text-primary);
}

.link-item-spotify .link-label {
  color: var(--text-primary);
}

.link-item-instagram .link-label {
  color: var(--text-primary);
}

.link-item-primary .link-label {
  color: var(--text-primary);
}

.link-item-personal .link-note {
  color: var(--prompt-branch);
}

.link-item-spotify .link-note {
  color: var(--prompt-branch);
}

.link-item-instagram .link-note {
  color: var(--prompt-branch);
}

.instagram-sparkles {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.instagram-sparkle {
  position: absolute;
  color: var(--prompt-branch);
  opacity: 0;
}

.instagram-sparkle-1 {
  top: 0.08em;
  left: 18%;
  font-size: 0.95em;
}

.instagram-sparkle-2 {
  top: 0.34em;
  left: 2%;
  font-size: 0.58em;
}

.instagram-sparkle-3 {
  top: 0.02em;
  right: 12%;
  font-size: 0.72em;
}

.instagram-sparkle-4 {
  top: 0.42em;
  right: 0%;
  font-size: 0.54em;
}

.instagram-sparkle-5 {
  bottom: 0.3em;
  left: 28%;
  font-size: 0.62em;
}

.instagram-sparkle-6 {
  bottom: 0.12em;
  left: 8%;
  font-size: 0.56em;
}

.instagram-sparkle-7 {
  bottom: 0.04em;
  right: 22%;
  font-size: 0.68em;
}

.instagram-sparkle-8 {
  top: 0.72em;
  right: 8%;
  font-size: 0.56em;
}

.instagram-sparkle-9 {
  top: 0;
  left: 42%;
  font-size: 0.46em;
}

.instagram-sparkle-10 {
  top: 0.26em;
  left: 58%;
  font-size: 0.5em;
}

.instagram-sparkle-11 {
  top: 0.18em;
  right: 32%;
  font-size: 0.48em;
}

.instagram-sparkle-12 {
  top: 0.64em;
  left: 48%;
  font-size: 0.42em;
}

.instagram-sparkle-13 {
  top: 0.3em;
  right: 48%;
  font-size: 0.46em;
}

.instagram-sparkle-14 {
  top: 0.54em;
  left: 22%;
  font-size: 0.42em;
}

.instagram-sparkle-15 {
  top: 0.52em;
  right: 36%;
  font-size: 0.44em;
}

.instagram-sparkle-16 {
  bottom: 0.08em;
  left: 52%;
  font-size: 0.46em;
}

.instagram-sparkle-17 {
  bottom: 0;
  left: 68%;
  font-size: 0.42em;
}

.instagram-sparkle-18 {
  bottom: 0.16em;
  right: 44%;
  font-size: 0.44em;
}

.instagram-sparkle-19 {
  bottom: 0.42em;
  right: 58%;
  font-size: 0.4em;
}

.instagram-sparkle-20 {
  bottom: 0.24em;
  left: 82%;
  font-size: 0.44em;
}

.link-item-primary .link-note {
  color: var(--text-muted);
}

.link-item-primary:hover .link-label,
.link-item-primary:focus-visible .link-label {
  color: var(--text-muted);
}

.link-item-primary:hover .link-note,
.link-item-primary:focus-visible .link-note {
  color: var(--text-primary);
}

.link-item-personal:hover .link-label,
.link-item-personal:focus-visible .link-label {
  color: var(--text-muted);
}

.link-item-personal:hover .link-note,
.link-item-personal:focus-visible .link-note {
  color: var(--text-primary);
}

.link-item-spotify:hover .link-label,
.link-item-spotify:focus-visible .link-label {
  color: var(--text-muted);
}

.link-item-spotify:hover .link-note,
.link-item-spotify:focus-visible .link-note {
  color: var(--text-primary);
  animation: spotify-pulse 2000ms cubic-bezier(0.33, 0, 0.2, 1) infinite;
}

.link-item-spotify:hover .link-note-text,
.link-item-spotify:focus-visible .link-note-text {
  animation: spotify-sway 1200ms cubic-bezier(0.33, 0, 0.2, 1) infinite;
}

.link-item-instagram:hover .link-label,
.link-item-instagram:focus-visible .link-label {
  color: var(--text-muted);
}

.link-item-instagram:hover .link-note,
.link-item-instagram:focus-visible .link-note {
  color: var(--text-primary);
}

.link-item-instagram:hover .instagram-sparkle-1,
.link-item-instagram:focus-visible .instagram-sparkle-1 {
  animation: instagram-twinkle 620ms ease-out 40ms 1;
}

.link-item-instagram:hover .instagram-sparkle-2,
.link-item-instagram:focus-visible .instagram-sparkle-2 {
  animation: instagram-twinkle-up-left 560ms ease-out 0ms 1;
}

.link-item-instagram:hover .instagram-sparkle-3,
.link-item-instagram:focus-visible .instagram-sparkle-3 {
  animation: instagram-twinkle-up-right 700ms ease-out 120ms 1;
}

.link-item-instagram:hover .instagram-sparkle-4,
.link-item-instagram:focus-visible .instagram-sparkle-4 {
  animation: instagram-twinkle-up-right 540ms ease-out 180ms 1;
}

.link-item-instagram:hover .instagram-sparkle-5,
.link-item-instagram:focus-visible .instagram-sparkle-5 {
  animation: instagram-twinkle-lower 620ms ease-out 90ms 1;
}

.link-item-instagram:hover .instagram-sparkle-6,
.link-item-instagram:focus-visible .instagram-sparkle-6 {
  animation: instagram-twinkle-down-left 720ms ease-out 150ms 1;
}

.link-item-instagram:hover .instagram-sparkle-7,
.link-item-instagram:focus-visible .instagram-sparkle-7 {
  animation: instagram-twinkle-down-right 760ms ease-out 210ms 1;
}

.link-item-instagram:hover .instagram-sparkle-8,
.link-item-instagram:focus-visible .instagram-sparkle-8 {
  animation: instagram-twinkle-up-right 600ms ease-out 60ms 1;
}

.link-item-instagram:hover .instagram-sparkle-9,
.link-item-instagram:focus-visible .instagram-sparkle-9 {
  animation: instagram-twinkle 560ms ease-out 30ms 1;
}

.link-item-instagram:hover .instagram-sparkle-10,
.link-item-instagram:focus-visible .instagram-sparkle-10 {
  animation: instagram-twinkle-up-right 540ms ease-out 95ms 1;
}

.link-item-instagram:hover .instagram-sparkle-11,
.link-item-instagram:focus-visible .instagram-sparkle-11 {
  animation: instagram-twinkle-up-right 580ms ease-out 145ms 1;
}

.link-item-instagram:hover .instagram-sparkle-12,
.link-item-instagram:focus-visible .instagram-sparkle-12 {
  animation: instagram-twinkle 520ms ease-out 65ms 1;
}

.link-item-instagram:hover .instagram-sparkle-13,
.link-item-instagram:focus-visible .instagram-sparkle-13 {
  animation: instagram-twinkle-up-left 610ms ease-out 190ms 1;
}

.link-item-instagram:hover .instagram-sparkle-14,
.link-item-instagram:focus-visible .instagram-sparkle-14 {
  animation: instagram-twinkle-down-left 560ms ease-out 40ms 1;
}

.link-item-instagram:hover .instagram-sparkle-15,
.link-item-instagram:focus-visible .instagram-sparkle-15 {
  animation: instagram-twinkle-down-right 590ms ease-out 120ms 1;
}

.link-item-instagram:hover .instagram-sparkle-16,
.link-item-instagram:focus-visible .instagram-sparkle-16 {
  animation: instagram-twinkle-lower 620ms ease-out 80ms 1;
}

.link-item-instagram:hover .instagram-sparkle-17,
.link-item-instagram:focus-visible .instagram-sparkle-17 {
  animation: instagram-twinkle-down-right 700ms ease-out 210ms 1;
}

.link-item-instagram:hover .instagram-sparkle-18,
.link-item-instagram:focus-visible .instagram-sparkle-18 {
  animation: instagram-twinkle-lower 640ms ease-out 140ms 1;
}

.link-item-instagram:hover .instagram-sparkle-19,
.link-item-instagram:focus-visible .instagram-sparkle-19 {
  animation: instagram-twinkle-down-left 530ms ease-out 20ms 1;
}

.link-item-instagram:hover .instagram-sparkle-20,
.link-item-instagram:focus-visible .instagram-sparkle-20 {
  animation: instagram-twinkle-down-right 760ms ease-out 170ms 1;
}

@keyframes spotify-pulse {
  0%,
  12%,
  25%,
  37%,
  50%,
  62%,
  75%,
  87%,
  100% {
    transform: translateY(0);
  }

  6%,
  31%,
  56%,
  81% {
    transform: translateY(-0.08em);
  }

  9%,
  34%,
  59%,
  84% {
    transform: translateY(0.03em);
  }
}

@keyframes spotify-sway {
  0%,
  100% {
    transform: rotate(0deg);
  }

  25% {
    transform: rotate(2deg);
  }

  75% {
    transform: rotate(-2deg);
  }
}

@keyframes instagram-twinkle {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.7);
  }

  35% {
    opacity: 1;
    transform: translate(0.12em, -0.18em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(0.18em, -0.28em) scale(0.85);
  }
}

@keyframes instagram-twinkle-up-left {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.7);
  }

  35% {
    opacity: 1;
    transform: translate(-0.12em, -0.18em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(-0.2em, -0.3em) scale(0.85);
  }
}

@keyframes instagram-twinkle-up-right {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.7);
  }

  35% {
    opacity: 1;
    transform: translate(0.14em, -0.18em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(0.22em, -0.3em) scale(0.85);
  }
}

@keyframes instagram-twinkle-lower {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.68);
  }

  35% {
    opacity: 1;
    transform: translate(0.06em, 0.14em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(0.1em, 0.28em) scale(0.82);
  }
}

@keyframes instagram-twinkle-down-left {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.68);
  }

  35% {
    opacity: 1;
    transform: translate(-0.08em, 0.14em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(-0.14em, 0.28em) scale(0.82);
  }
}

@keyframes instagram-twinkle-down-right {
  0% {
    opacity: 0;
    transform: translate(0, 0) scale(0.68);
  }

  35% {
    opacity: 1;
    transform: translate(0.08em, 0.14em) scale(1);
  }

  100% {
    opacity: 0;
    transform: translate(0.14em, 0.28em) scale(0.82);
  }
}
</style>
