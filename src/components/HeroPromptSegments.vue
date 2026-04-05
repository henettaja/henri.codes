<template>
  <div class="hero-prompt">
    <span class="prompt-path">henri.codes </span>
    <span class="prompt-connector">on </span>
    <span class="prompt-branch">⎇ main </span>
    <span class="prompt-connector">via </span>
    <span class="prompt-node">
      <span class="prompt-node-icon">⬢</span>
      <span>24.14.1</span>
    </span>
  </div>
</template>

<style scoped>
.hero-prompt {
  line-height: 1.4;
  font-size: 2rem;
  user-select: none;
}

.prompt-path {
  color: var(--prompt-path);
  font-weight: 600;
}

.prompt-connector {
  color: var(--text-primary);
}

.prompt-branch {
  color: var(--prompt-branch);
  font-weight: 600;
  white-space: nowrap;
}

.prompt-node {
  display: inline-flex;
  align-items: baseline;
  gap: 0.35em;
  color: var(--prompt-node);
  font-weight: 600;
  white-space: nowrap;
}

.prompt-node-icon {
  font-size: 1.25em;
  line-height: 1;
}
</style>
