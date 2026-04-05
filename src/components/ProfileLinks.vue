<script setup lang="ts">
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
      <ul class="link-list">
        <li v-for="(link, index) in links" :key="link.href">
          <span class="link-branch" aria-hidden="true">
            {{ index === links.length - 1 ? '└─' : '├─' }}
          </span>
          <a
            :href="link.href"
            target="_blank"
            rel="noreferrer"
            :class="['link-item', `link-item-${link.kind}`]"
          >
            <span class="link-label">{{ link.label }}</span>
            <span class="link-note">{{ link.note }}</span>
          </a>
        </li>
      </ul>
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
  display: grid;
  gap: 1rem;
  margin-top: 1rem;
}

.link-list {
  display: grid;
  gap: 1rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.link-list li {
  display: grid;
  grid-template-columns: 2.5rem minmax(0, 1fr);
  align-items: start;
  column-gap: 0.75rem;
}

.link-branch {
  color: var(--text-muted);
  width: 2.5rem;
  flex: 0 0 auto;
}

.link-list a {
  display: inline-flex;
  align-items: center;
  flex-wrap: wrap;
  min-height: 0;
  padding: 0;
  transition: color 140ms ease;
  color: var(--text-primary);
  gap: 0.75rem;
  min-width: 0;
}

.link-list a:hover,
.link-list a:focus-visible {
  color: var(--text-muted);
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

.link-item-personal .link-note {
  color: var(--prompt-glyph);
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
