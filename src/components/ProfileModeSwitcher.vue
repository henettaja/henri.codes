<script setup lang="ts">
type ProfileMode = 'dev' | 'human'

defineProps<{
  activeMode: ProfileMode
  modeOptions: ReadonlyArray<{
    id: ProfileMode
    label: string
  }>
}>()

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
      :class="['mode-button', { active: activeMode === mode.id }]"
      :aria-pressed="activeMode === mode.id"
      @click="emit('update:activeMode', mode.id)"
    >
      <span>{{ mode.label }}</span>
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
  color: var(--text-muted);
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
  color: var(--text-muted);
  cursor: pointer;
  transition: color 140ms ease;
}

.mode-button:hover,
.mode-button:focus-visible,
.mode-button.active {
  color: var(--text-primary);
}

.mode-divider {
  color: var(--text-muted);
}
</style>
