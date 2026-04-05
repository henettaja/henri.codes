<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import HeroPromptSegments from './HeroPromptSegments.vue'
import TypedIntro from './TypedIntro.vue'

const props = defineProps<{
  portraitSrc: string
  portraitAlt: string
  contentInset?: string
}>()

const emit = defineEmits<{
  ready: []
}>()

const introSwapOpacityMs = 500
const introSwapTransformMs = 1000
const introExitFadeDelayMs = 120
const introCommandPauseMs = 180
const introProcessingMs = 1580
const introSecondLineRevealMs = 220
const processingSpinnerStepMs = 80
const helloStartDelayMs = introSwapTransformMs + 500
const portraitRevealDelayMs = introSwapTransformMs + 500

const introSwapOpacityDuration = `${introSwapOpacityMs}ms`
const introSwapTransformDuration = `${introSwapTransformMs}ms`
const introExitFadeDelayDuration = `${introExitFadeDelayMs}ms`
const introSecondLineRevealDuration = `${introSecondLineRevealMs}ms`
const portraitRevealDelayDuration = `${portraitRevealDelayMs}ms`

const isReady = ref(false)
const isProcessing = ref(false)
const spinnerFrameIndex = ref(0)

const spinnerFrames = ['⣾', '⣷', '⣯', '⣟', '⡿', '⢿', '⣻', '⣽']
const spinnerFrame = computed(() => spinnerFrames[spinnerFrameIndex.value] ?? spinnerFrames[0])

let processingTimerId: number | null = null
let spinnerTimerId: number | null = null
let commandPauseTimerId: number | null = null
const finishIntro = () => {
  if (isReady.value) {
    return
  }

  isReady.value = true
  emit('ready')
}

const handleInitComplete = () => {
  if (isReady.value) {
    return
  }

  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    finishIntro()
    return
  }

  commandPauseTimerId = window.setTimeout(() => {
    isProcessing.value = true
    spinnerFrameIndex.value = 0

    spinnerTimerId = window.setInterval(() => {
      spinnerFrameIndex.value =
        (spinnerFrameIndex.value + 1) % spinnerFrames.length
    }, processingSpinnerStepMs)

    processingTimerId = window.setTimeout(() => {
      isProcessing.value = false

      if (spinnerTimerId !== null) {
        window.clearInterval(spinnerTimerId)
        spinnerTimerId = null
      }

      finishIntro()
    }, introProcessingMs)
  }, introCommandPauseMs)
}

onMounted(() => {
  const prefersReducedMotion = window.matchMedia(
    '(prefers-reduced-motion: reduce)',
  ).matches

  if (prefersReducedMotion) {
    finishIntro()
  }
})

onBeforeUnmount(() => {
  if (commandPauseTimerId !== null) {
    window.clearTimeout(commandPauseTimerId)
  }

  if (processingTimerId !== null) {
    window.clearTimeout(processingTimerId)
  }

  if (spinnerTimerId !== null) {
    window.clearInterval(spinnerTimerId)
  }
})
</script>

<template>
  <section
    class="hero"
    :class="{ 'is-ready': isReady }"
    :style="{ '--content-inset': props.contentInset ?? '1.4rem' }"
    aria-label="Introduction"
  >
    <div class="hero-portrait" :class="{ 'is-visible': isReady }" aria-hidden="true">
      <div class="portrait-frame hero-portrait-frame">
        <img
          class="portrait-image"
          :src="portraitSrc"
          :alt="portraitAlt"
        />
      </div>
    </div>

    <div class="hero-copy-stage">
      <div class="hero-copy hero-copy-init" :class="{ 'is-hidden': isReady }">
        <HeroPromptSegments />
        <div class="command-stage">
          <div class="hero-command">
            <span class="prompt-glyph">❯</span>
            <TypedIntro
              as="span"
              class="hero-title"
              text="./init_site.sh"
              :step-ms="55"
              @complete="handleInitComplete"
            />
          </div>
          <div
            class="hero-command command-slot-secondary"
            :class="{ visible: isProcessing || isReady }"
          >
            <span
              v-if="isProcessing || isReady"
              class="processing-spinner"
              aria-hidden="true"
            >{{ spinnerFrame }}</span>
          </div>
        </div>
      </div>

      <div class="hero-copy hero-copy-final" :class="{ 'is-visible': isReady }">
        <HeroPromptSegments />
        <div class="hero-command">
          <span class="prompt-glyph">❯</span>
          <TypedIntro
            v-if="isReady"
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
</template>

<style scoped>
.hero {
  display: grid;
  align-items: center;
  gap: 4rem;
  padding: 1.25rem 0 0.75rem;
}

.hero-portrait {
  display: flex;
  visibility: hidden;
}

.hero-portrait.is-visible {
  visibility: visible;
}

.hero-portrait-frame {
  width: min(100%, 22.5rem);
  opacity: 0;
  filter: brightness(1.4) contrast(1.08);
}

.hero-portrait.is-visible .hero-portrait-frame {
  animation:
    portrait-flicker 2600ms linear v-bind(portraitRevealDelayDuration) 1 both,
    portrait-glow-settle 2700ms ease-out v-bind(portraitRevealDelayDuration) 1
      both;
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
  align-content: start;
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
  padding-top: 0.75rem;
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
  position: relative;
  display: block;
  min-height: 5.1rem;
}

.command-slot-secondary {
  position: absolute;
  left: 0;
  top: 3.75rem;
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

.processing-spinner {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 1ch;
  aspect-ratio: 1;
  color: var(--text-muted);
  line-height: 1;
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

@keyframes portrait-flicker {
  0% {
    opacity: 0;
    filter: brightness(1.75) contrast(1.15);
  }

  4% {
    opacity: 0.58;
    filter: brightness(0.5) contrast(1.02);
  }

  9% {
    opacity: 0.04;
    filter: brightness(1.95) contrast(1.22);
  }

  15% {
    opacity: 0.92;
    filter: brightness(0.84) contrast(1.06);
  }

  19% {
    opacity: 0.2;
    filter: brightness(1.42) contrast(1.15);
  }

  28% {
    opacity: 0.98;
    filter: brightness(0.88) contrast(1.05);
  }

  35% {
    opacity: 0.62;
    filter: brightness(1.16) contrast(1.08);
  }

  43% {
    opacity: 0.86;
    filter: brightness(0.95) contrast(1.03);
  }

  52% {
    opacity: 0.18;
    filter: brightness(1.36) contrast(1.12);
  }

  63% {
    opacity: 1;
    filter: brightness(0.91) contrast(1.02);
  }

  71% {
    opacity: 0.74;
    filter: brightness(1.08) contrast(1.04);
  }

  82% {
    opacity: 0.96;
    filter: brightness(0.98) contrast(1.01);
  }

  100% {
    opacity: 1;
    filter: brightness(1) contrast(1);
  }
}

@keyframes portrait-glow-settle {
  0% {
    box-shadow: 0 0 0 rgba(255, 255, 255, 0);
  }

  18% {
    box-shadow: 0 0 2.4rem rgba(255, 255, 255, 0.08);
  }

  100% {
    box-shadow: 0 0 0 rgba(255, 255, 255, 0);
  }
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
}

@media (prefers-reduced-motion: reduce) {
  .hero-copy-init,
  .hero-copy-final,
  .hero-portrait,
  .command-slot-secondary {
    transition: none;
  }

  .hero-portrait.is-visible .hero-portrait-frame {
    animation: none;
  }
}
</style>
