<script setup lang="ts">
defineProps<{
  activeTab: string
  sections: ReadonlyArray<{
    id: string
    label: string
    title: string
    body: string
  }>
}>()

const emit = defineEmits<{
  'update:activeTab': [tabId: string]
}>()
</script>

<template>
  <article class="content-section content-tabs">
    <div class="tab-list" role="tablist" aria-label="Profile sections">
      <button
        v-for="section in sections"
        :key="section.id"
        :class="['tab-button', { active: activeTab === section.id }]"
        :aria-selected="activeTab === section.id"
        :tabindex="activeTab === section.id ? 0 : -1"
        role="tab"
        type="button"
        @click="emit('update:activeTab', section.id)"
      >
        {{ section.label }}
      </button>
    </div>

    <div
      v-for="section in sections"
      v-show="activeTab === section.id"
      :key="section.id"
      class="tab-panel"
      role="tabpanel"
    >
      <h3>{{ section.title }}</h3>
      <p>{{ section.body }}</p>
    </div>
  </article>
</template>

<style scoped>
.content-section {
  display: grid;
  gap: 1.5rem;
  padding: 0;
}

.content-tabs {
  align-content: start;
}

.tab-list {
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
}

.tab-button {
  border: 0;
  background: transparent;
  color: var(--text-muted);
  padding: 0.25rem 0;
  min-height: 0;
  cursor: pointer;
  transition:
    color 140ms ease,
    text-decoration-color 140ms ease;
  text-decoration: underline;
  text-decoration-color: transparent;
  text-underline-offset: 0.25em;
}

.tab-button:hover,
.tab-button:focus-visible,
.tab-button.active {
  color: var(--text-primary);
  text-decoration-color: currentColor;
}

.tab-panel {
  display: grid;
  gap: 1rem;
  min-height: 14rem;
  align-content: start;
}

.tab-panel h3 {
  font-size: 2.5rem;
}
</style>
