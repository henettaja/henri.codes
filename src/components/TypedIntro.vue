<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'

const props = defineProps<{
  text: string
  as?: string
  startDelay?: number
  stepMs?: number
}>()

const emit = defineEmits<{
  complete: []
}>()

const displayText = ref('')
const isComplete = ref(false)

let timerId: number | null = null
let startTimeoutId: number | null = null

const tagName = computed(() => props.as ?? 'span')

const finishTyping = () => {
  if (isComplete.value) {
    return
  }

  displayText.value = props.text
  isComplete.value = true
  emit('complete')
}

onMounted(() => {
  const prefersReducedMotion = window.matchMedia(
    '(prefers-reduced-motion: reduce)',
  ).matches

  if (prefersReducedMotion) {
    finishTyping()
    return
  }

  const beginTyping = () => {
    let index = 0

    timerId = window.setInterval(() => {
      index += 1
      displayText.value = props.text.slice(0, index)

      if (index >= props.text.length) {
        if (timerId !== null) {
          window.clearInterval(timerId)
        }

        timerId = null
        finishTyping()
      }
    }, props.stepMs ?? 90)
  }

  startTimeoutId = window.setTimeout(beginTyping, props.startDelay ?? 0)
})

onBeforeUnmount(() => {
  if (startTimeoutId !== null) {
    window.clearTimeout(startTimeoutId)
  }

  if (timerId !== null) {
    window.clearInterval(timerId)
  }
})
</script>

<template>
  <component :is="tagName" class="typed-intro">
    <span>{{ displayText }}</span>
    <span class="cursor" :class="{ complete: isComplete }" aria-hidden="true" />
  </component>
</template>

<style scoped>
.typed-intro {
  display: inline-flex;
  align-items: baseline;
  margin: 0;
  font: inherit;
  color: inherit;
  letter-spacing: inherit;
}

.cursor {
  width: 0.6ch;
  height: 0.95em;
  margin-left: 0.08em;
  transform: translateY(0.08em);
  background: currentColor;
  animation: blink 1s steps(1, end) infinite;
}

.cursor.complete {
  margin-left: 0.14em;
}

@keyframes blink {
  0%,
  49% {
    opacity: 1;
  }

  50%,
  100% {
    opacity: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .cursor {
    animation-duration: 1.6s;
  }
}
</style>
