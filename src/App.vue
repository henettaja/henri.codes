<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import heroDithered from './assets/hero-portrait.png'
import HeroPromptSegments from './components/HeroPromptSegments.vue'
import ProfileLinks from './components/ProfileLinks.vue'
import ProfileModeSwitcher from './components/ProfileModeSwitcher.vue'
import ProfileTabs from './components/ProfileTabs.vue'
import TreeList from './components/TreeList.vue'
import TreeListItem from './components/TreeListItem.vue'
import TypedIntro from './components/TypedIntro.vue'

type ProfileMode = 'dev' | 'human'

const introRevealMs = 360
const introSwapOpacityMs = 500
const introSwapTransformMs = 1000
const introExitFadeDelayMs = 120
const introProcessingMs = 680
const introSecondLineRevealMs = 220
const processingDotsCycleMs = 900
const processingDotsStaggerMs = 120
const helloStartDelayMs = Math.round(introSwapTransformMs * 0.22)
const contentRevealDelayMs = introSwapTransformMs
const portraitRevealDelayMs = contentRevealDelayMs + 120

const introRevealDuration = `${introRevealMs}ms`
const introSwapOpacityDuration = `${introSwapOpacityMs}ms`
const introSwapTransformDuration = `${introSwapTransformMs}ms`
const introExitFadeDelayDuration = `${introExitFadeDelayMs}ms`
const introSecondLineRevealDuration = `${introSecondLineRevealMs}ms`
const processingDotsCycleDuration = `${processingDotsCycleMs}ms`
const processingDotsSecondDelay = `${processingDotsStaggerMs}ms`
const processingDotsThirdDelay = `${processingDotsStaggerMs * 2}ms`
const portraitRevealDelayDuration = `${portraitRevealDelayMs}ms`
const contentRevealDelayDuration = `${contentRevealDelayMs}ms`

const activeTab = ref('work')
const activeMode = ref<ProfileMode>('dev')
const isIntroReady = ref(false)
const isProcessingIntro = ref(false)

const modeOptions = [
  {
    id: 'dev',
    label: 'dev',
  },
  {
    id: 'human',
    label: 'human',
  },
] as const

const profileDetails = [
  ['profile', 'frontend focused full-stack developer'],
  ['focus', 'TypeScript-based stacks'],
  ['location', 'Helsinki'],
] as const

const sectionsByMode = {
  dev: {
    sections: [
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I enjoy modern monorepositories, declarative, reactive and functional paradigms, and projects that aim to leave the world better than they found it. I enjoy the challenge of figuring out the right technical structure for each scenario, whether that means a local implementation choice or the shape of a larger system. I believe product quality and code quality are inseparable.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I like working with teams that make calculated decisions through open discussion, even friendly debate. The best technical direction usually comes from people caring enough to challenge each other’s thinking. I thrive in environments where ambition, curiosity, and technical taste are taken seriously.',
        details: [],
      },
    ],
  },
  human: {
    sections: [
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I build and maintain production applications in client projects, focusing on maintainable front-end architecture. I’ve contributed across the stack and worked directly with clients to deliver production systems while taking ownership of technical direction. I see technical quality as part of delivering a better product, not as a separate concern. I’m especially drawn to projects that aim to leave the world better than they found it. I’m also regularly thanked for enjoyable collaboration by non-technical clients.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I like working with teams that make calculated decisions based on open discussion, even friendly debate. It cultivates clarity and trust, which leads to better collaboration and a better product.',
        details: [],
      },
    ],
  },
} as const

const profileLinks = [
  {
    href: 'https://www.linkedin.com/in/henrivaisanen/',
    label: 'LinkedIn',
    note: 'career',
    kind: 'primary',
  },
  {
    href: 'https://github.com/henettaja',
    label: 'GitHub',
    note: 'code',
    kind: 'primary',
  },
  {
    href: 'https://open.spotify.com/user/henettaja',
    label: 'Spotify',
    note: 'currently playing',
    kind: 'personal',
  },
  {
    href: 'https://instagram.com/henettaja',
    label: 'Instagram',
    note: 'offline traces',
    kind: 'personal',
  },
]

let processingTimerId: number | null = null

const handleInitComplete = () => {
  if (isIntroReady.value) {
    return
  }

  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    isIntroReady.value = true
    return
  }

  isProcessingIntro.value = true

  processingTimerId = window.setTimeout(() => {
    isProcessingIntro.value = false
    isIntroReady.value = true
  }, introProcessingMs)
}

onMounted(() => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    isIntroReady.value = true
  }
})

onBeforeUnmount(() => {
  if (processingTimerId !== null) {
    window.clearTimeout(processingTimerId)
  }
})
</script>

<template>
  <main class="page-shell">
    <section class="hero" :class="{ 'is-ready': isIntroReady }" aria-label="Introduction">
      <div class="hero-portrait" :class="{ 'is-visible': isIntroReady }" aria-hidden="true">
        <div class="portrait-frame hero-portrait-frame">
          <img
            class="portrait-image"
            :src="heroDithered"
            alt="Portrait of Henri Vaisanen"
          />
        </div>
      </div>

      <div class="hero-copy-stage">
        <div class="hero-copy hero-copy-init" :class="{ 'is-hidden': isIntroReady }">
          <HeroPromptSegments />
          <div class="command-stage" :class="{ 'has-second-line': isProcessingIntro }">
            <div class="hero-command">
              <span class="prompt-glyph">❯</span>
              <TypedIntro
                as="span"
                class="hero-title"
                text="./init_site.sh"
                :step-ms="85"
                @complete="handleInitComplete"
              />
            </div>
            <div
              class="hero-command command-slot-secondary"
              :class="{ visible: isProcessingIntro }"
            >
              <span
                v-if="isProcessingIntro"
                class="processing-dots"
                aria-hidden="true"
              >
                <span>.</span>
                <span>.</span>
                <span>.</span>
              </span>
            </div>
          </div>
        </div>

        <div class="hero-copy hero-copy-final" :class="{ 'is-visible': isIntroReady }">
          <HeroPromptSegments />
          <div class="hero-command">
            <span class="prompt-glyph">❯</span>
            <TypedIntro
              v-if="isIntroReady"
              id="intro-title"
              as="span"
              class="hero-title"
              text="hello world!"
              :start-delay="helloStartDelayMs"
              :step-ms="90"
            />
          </div>
        </div>
      </div>
    </section>

    <Transition name="intro-reveal">
      <section
        v-if="isIntroReady"
        class="content-area"
        aria-labelledby="info-title"
      >
        <div class="section-copy content-heading">
          <div class="identity-panel" aria-label="Profile header">
            <span class="identity-panel-label">whoami</span>
            <div class="identity-panel-body">
              <div class="heading-row">
                <h2 id="info-title">henri_väisänen</h2>
                <ProfileModeSwitcher
                  :active-mode="activeMode"
                  :mode-options="modeOptions"
                  @update:active-mode="activeMode = $event"
                />
              </div>
              <p class="identity-tagline">
                I write code I can stand behind, for causes I can stand behind.
              </p>
              <div class="identity-output">
                <h3 class="identity-section-title">./summary</h3>
                <div class="identity-detail-tree" aria-label="Profile details">
                  <TreeList>
                    <TreeListItem
                      v-for="([term, description], index) in profileDetails"
                      :key="term"
                      :is-last="index === profileDetails.length - 1"
                    >
                      <dl class="identity-detail-entry">
                        <dt>{{ term }}</dt>
                        <dd>{{ description }}</dd>
                      </dl>
                    </TreeListItem>
                  </TreeList>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="content-grid" aria-label="Introduction">
          <ProfileTabs
            :active-tab="activeTab"
            :sections="sectionsByMode[activeMode].sections"
            @update:active-tab="activeTab = $event"
          />

          <ProfileLinks :links="profileLinks" />
        </div>
      </section>
    </Transition>
  </main>
</template>

<style scoped>
.page-shell {
  --content-inset: 1.4rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 6rem;
  width: min(147.5rem, calc(100% - 6rem));
  margin: 0 auto;
  padding: 2.5rem 0 4rem;
}

.hero {
  display: grid;
  align-items: center;
  gap: 4rem;
  padding: 1.25rem 0 0.75rem;
}

.hero-portrait {
  display: flex;
  opacity: 0;
  visibility: hidden;
  transform: translateY(1rem);
  transition:
    opacity v-bind(introRevealDuration) ease,
    transform v-bind(introRevealDuration) ease,
    visibility 0s linear v-bind(introRevealDuration);
  transition-delay:
    v-bind(portraitRevealDelayDuration),
    v-bind(portraitRevealDelayDuration),
    calc(v-bind(portraitRevealDelayDuration) + v-bind(introRevealDuration));
}

.hero-portrait.is-visible {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
  transition:
    opacity v-bind(introRevealDuration) ease,
    transform v-bind(introRevealDuration) ease,
    visibility 0s linear 0s;
  transition-delay:
    v-bind(portraitRevealDelayDuration),
    v-bind(portraitRevealDelayDuration),
    v-bind(portraitRevealDelayDuration);
}

.hero-portrait-frame {
  width: min(100%, 22.5rem);
}

.hero-copy-stage {
  position: relative;
  min-height: 10rem;
  padding: 2rem 0;
  margin: -2rem 0;
  overflow: visible;
}

.hero-copy {
  display: grid;
  gap: 0.5rem;
  align-content: center;
  box-sizing: border-box;
  padding-left: var(--content-inset);
}

.hero-copy-init,
.hero-copy-final {
  position: absolute;
  inset: 0;
  transition-property: opacity, transform;
  transition-duration:
    v-bind(introSwapOpacityDuration),
    v-bind(introSwapTransformDuration);
  transition-timing-function: ease-in-out, ease-in-out;
}

.hero-copy-init {
  opacity: 1;
  transform: translateY(0);
}

.hero-copy-init.is-hidden {
  opacity: 0;
  transform: translateY(-8rem);
  transition-delay: v-bind(introExitFadeDelayDuration), 0ms;
}

.hero-copy-final {
  opacity: 0;
  transform: translateY(8rem);
  transition-delay: 0ms, 0ms;
}

.hero-copy-final.is-visible {
  opacity: 1;
  transform: translateY(0);
  transition-delay: v-bind(introExitFadeDelayDuration), 0ms;
}

.hero-command {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 2rem;
  user-select: none;
}

.section-label {
  text-transform: uppercase;
  letter-spacing: 0.16em;
  color: var(--text-muted);
  font-size: 1.75rem;
}

.prompt-glyph {
  display: inline-block;
  width: 1ch;
  color: var(--prompt-glyph);
}

.hero-title {
  font-size: 2rem;
  line-height: 1.1;
  text-transform: lowercase;
}

.command-stage {
  display: grid;
  gap: 0.5rem;
  min-height: 3rem;
}

.command-stage.has-second-line {
  min-height: 6.5rem;
}

.command-slot-secondary {
  opacity: 0;
  transform: translateY(-0.4rem);
  transition:
    opacity v-bind(introSecondLineRevealDuration) ease,
    transform v-bind(introSecondLineRevealDuration) ease;
}

.command-slot-secondary.visible {
  opacity: 1;
  transform: translateY(0);
}

.processing-dots {
  display: inline-flex;
  gap: 0.1em;
  margin-left: 0.2em;
  color: var(--text-muted);
}

.processing-dots span {
  animation: processing-dot v-bind(processingDotsCycleDuration) ease-in-out infinite;
}

.processing-dots span:nth-child(2) {
  animation-delay: v-bind(processingDotsSecondDelay);
}

.processing-dots span:nth-child(3) {
  animation-delay: v-bind(processingDotsThirdDelay);
}

@keyframes processing-dot {
  0%,
  100% {
    opacity: 0.3;
  }

  50% {
    opacity: 1;
  }
}

.content-grid {
  display: grid;
  gap: 4rem;
  padding-left: var(--content-inset);
}

.content-area {
  display: grid;
  gap: 5rem;
}

.intro-reveal-enter-active {
  transition:
    opacity v-bind(introRevealDuration) ease,
    transform v-bind(introRevealDuration) ease;
  transition-delay: v-bind(contentRevealDelayDuration);
}

.intro-reveal-enter-from,
.intro-reveal-leave-to {
  opacity: 0;
  transform: translateY(1rem);
}

.intro-reveal-enter-to,
.intro-reveal-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.content-section {
  display: grid;
  gap: 1.5rem;
  padding: 0;
}

.section-copy {
  display: grid;
  gap: 1.25rem;
}

.identity-panel {
  position: relative;
  display: inline-block;
  justify-self: start;
  width: fit-content;
  max-width: 100%;
  padding: 2rem 1.4rem 1.4rem;
  border: 1px solid var(--panel-border-strong);
  border-radius: 1.4rem;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.025) 0%,
    rgba(255, 255, 255, 0.01) 100%
  );
}

.identity-panel-body {
  display: grid;
  gap: 1.25rem;
}

.identity-tagline {
  color: var(--text-primary);
}

.identity-detail-tree {
  margin-top: 0.25rem;
}

.identity-output {
  display: grid;
  gap: 0.5rem;
}

.identity-section-title {
  color: var(--text-muted);
  font-size: 1.75rem;
  font-weight: 500;
}

.identity-detail-entry {
  display: grid;
  grid-template-columns: 9.5rem minmax(0, 1fr);
  gap: 3rem;
  margin: 0;
}

.identity-detail-entry dt {
  color: var(--text-muted);
}

.identity-detail-entry dd {
  margin: 0;
  color: var(--text-primary);
}

.identity-panel-label {
  position: absolute;
  top: 0;
  left: 1.2rem;
  transform: translateY(-50%);
  padding: 0 0.75rem;
  background: var(--bg);
  color: var(--prompt-path);
  line-height: 1;
}

.heading-row {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 1rem 2rem;
}

.portrait-frame {
  position: relative;
  overflow: hidden;
  background: transparent;
}

.portrait-image {
  width: 100%;
  aspect-ratio: 1 / 0.86;
  object-fit: cover;
  object-position: center top;
  image-rendering: pixelated;
}

@media (min-width: 500px) and (max-width: 799px) {
  .hero-copy-stage {
    min-height: 10rem;
    padding: 1.5rem 0;
    margin: -1.5rem 0;
  }

  .hero-title {
    font-size: 2.5rem;
  }

  .hero {
    grid-template-columns: minmax(14rem, 20rem) minmax(0, 1fr);
    gap: 4rem;
    padding: 1rem 0 0.75rem;
    min-height: 32vh;
  }

  .hero-portrait {
    justify-content: flex-start;
  }

  .hero-portrait-frame {
    width: min(100%, 22rem);
  }

  .hero-copy {
    gap: 1rem;
  }
}

@media (min-width: 800px) {
  .page-shell {
    gap: 5rem;
    width: min(147.5rem, calc(100% - 12rem));
    padding: 4.5rem 0 5rem;
  }

  .hero-copy-stage {
    min-height: 10rem;
  }

  .hero-title {
    font-size: 2.5rem;
  }

  .hero {
    grid-template-columns: minmax(16.25rem, 23.75rem) minmax(0, 1fr);
    gap: 5rem;
    padding: 2.5rem 0 1.5rem;
    min-height: 42vh;
  }

  .hero-portrait {
    justify-content: flex-start;
  }

  .hero-portrait-frame {
    width: min(100%, 26.25rem);
  }

  .hero-copy {
    gap: 1rem;
  }

  .content-grid {
    grid-template-columns: minmax(0, 1fr) minmax(26rem, 34rem);
    align-items: start;
  }

  .links-panel {
    align-content: start;
    min-width: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .intro-reveal-enter-active {
    transition: none;
  }

  .hero-copy-init,
  .hero-copy-final,
  .hero-portrait,
  .command-slot-secondary {
    transition: none;
  }

  .processing-dots span {
    animation: none;
    opacity: 1;
  }
}
</style>
