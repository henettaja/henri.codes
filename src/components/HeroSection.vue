<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
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

const isReady = ref(false)
const isProcessing = ref(false)

let processingTimerId: number | null = null

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

  isProcessing.value = true

  processingTimerId = window.setTimeout(() => {
    isProcessing.value = false
    finishIntro()
  }, introProcessingMs)
}

onMounted(() => {
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    finishIntro()
  }
})

onBeforeUnmount(() => {
  if (processingTimerId !== null) {
    window.clearTimeout(processingTimerId)
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
        <div class="command-stage" :class="{ 'has-second-line': isProcessing }">
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
            :class="{ visible: isProcessing }"
          >
            <span
              v-if="isProcessing"
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

  .processing-dots span {
    animation: none;
    opacity: 1;
  }
}
</style>
