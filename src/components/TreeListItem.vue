<script setup lang="ts">
defineProps<{
  isLast?: boolean
}>()
</script>

<template>
  <li :class="{ 'is-last': isLast }">
    <span class="tree-branch" aria-hidden="true"></span>
    <div class="tree-content">
      <slot></slot>
    </div>
  </li>
</template>

<style scoped>
li {
  position: relative;
  display: grid;
  grid-template-columns: 2.9rem minmax(0, 1fr);
  align-items: start;
  column-gap: 0.75rem;
}

.tree-branch {
  position: relative;
  width: 2.9rem;
  min-height: 100%;
}

.tree-branch::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0.95rem;
  width: 1px;
  background: var(--tree-color);
}

.tree-branch::after {
  content: '';
  position: absolute;
  top: 1.95rem;
  left: 0.95rem;
  width: 1.25rem;
  height: 1px;
  background: var(--tree-color);
}

li.is-last .tree-branch::before {
  bottom: auto;
  height: 1.95rem;
}

.tree-content {
  min-width: 0;
  padding: 0.5rem 0;
}
</style>
