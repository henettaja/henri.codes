<template>
  <div class="tree-list-wrapper">
    <ul class="tree-list">
      <slot></slot>
    </ul>
  </div>
</template>

<style scoped>
.tree-list-wrapper {
  --tree-color: var(--text-muted);
  display: grid;
  gap: 1rem;
}

.tree-list {
  display: grid;
  list-style: none;
  padding: 0;
  margin: 0;
}
</style>
