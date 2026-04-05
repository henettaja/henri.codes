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
          <span class="link-label">{{ link.label }}</span>
          <span class="link-note">{{ link.note }}</span>
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
  text-decoration: underline;
  text-decoration-color: currentColor;
  text-underline-offset: 0.25em;
}

.link-note {
  color: var(--text-muted);
  font-size: 1.5rem;
}

.link-item-personal .link-label {
  color: var(--text-primary);
}

.link-item-primary .link-label {
  color: var(--text-primary);
}

.link-item-personal .link-note {
  color: var(--prompt-glyph);
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
</style>
