<script setup lang="ts">
import { ref } from 'vue'
import heroDithered from './assets/hero-portrait.png'
import ProfileLinks from './components/ProfileLinks.vue'
import ProfileModeSwitcher from './components/ProfileModeSwitcher.vue'
import TypedIntro from './components/TypedIntro.vue'

type ProfileMode = 'dev' | 'human'

const activeTab = ref('about')
const activeMode = ref<ProfileMode>('dev')

const modeOptions = [
  {
    id: 'dev',
    label: 'dev',
  },
  {
    id: 'human',
    label: 'human',
  },
] as const

const sectionsByMode = {
  dev: {
    sections: [
      {
        id: 'about',
        label: './about',
        title: '',
        body: '',
        details: [
          ['profile', 'frontend focused full-stack developer'],
          ['tech', 'TypeScript-based stacks'],
          ['location', 'Helsinki'],
        ],
      },
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I enjoy modern monorepositories, declarative, reactive and functional paradigms, and projects that aim to leave the world better than they found it.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I like working with teams that make calculated decisions based on open discussion, even friendly debate.',
        details: [],
      },
    ],
  },
  human: {
    sections: [
      {
        id: 'about',
        label: './about',
        title: '',
        body: '',
        details: [
          ['profile', 'frontend focused full-stack developer'],
          ['focus', 'shipping reliable TypeScript products'],
          ['location', 'Helsinki'],
        ],
      },
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I work best in teams that trust engineers with real ownership: shaping solutions, building client-facing products, and carrying ideas into production with a high bar for quality.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'My approach is direct and accountable. I value clear communication, thoughtful technical decisions, and software that improves the experience for both users and the people maintaining it.',
        details: [],
      },
    ],
  },
} as const

const profileLinks = [
  {
    href: 'https://www.linkedin.com/in/henrivaisanen/',
    label: 'LinkedIn',
    note: 'professional',
    kind: 'primary',
  },
  {
    href: 'https://github.com/henettaja',
    label: 'GitHub',
    note: 'code',
    kind: 'primary',
  },
  {
    href: 'https://open.spotify.com/user/henettaja',
    label: 'Spotify',
    note: 'currently playing',
    kind: 'personal',
  },
  {
    href: 'https://instagram.com/henettaja',
    label: 'Instagram',
    note: 'offline traces',
    kind: 'personal',
  },
]
</script>

<template>
  <main class="page-shell">
    <section class="hero" aria-labelledby="intro-title">
      <div class="hero-portrait">
        <div class="portrait-frame hero-portrait-frame">
          <img
            class="portrait-image"
            :src="heroDithered"
            alt="Portrait of Henri Vaisanen"
          />
        </div>
      </div>

      <div class="hero-copy">
        <div class="hero-prompt">
          <span class="prompt-path">henri.codes</span>
          <span class="prompt-connector">on</span>
          <span class="prompt-branch">main</span>
        </div>
        <div class="hero-command">
          <span class="prompt-glyph">❯</span>
          <TypedIntro
            id="intro-title"
            class="hero-title"
            text="hello world!"
          />
        </div>
      </div>
    </section>

    <section class="content-area" aria-labelledby="info-title">
      <div class="section-copy content-heading">
        <h2 id="info-title">henri_väisänen</h2>
        <p>I write code I can stand behind, for causes I can stand behind.</p>
        <ProfileModeSwitcher
          :active-mode="activeMode"
          :mode-options="modeOptions"
          @update:active-mode="activeMode = $event"
        />
      </div>

      <div class="content-grid" aria-label="Introduction">
        <article class="content-section content-tabs">

        <div class="tab-list" role="tablist" aria-label="Profile sections">
          <button
            v-for="section in sectionsByMode[activeMode].sections"
            :key="section.id"
            :class="['tab-button', { active: activeTab === section.id }]"
            :aria-selected="activeTab === section.id"
            :tabindex="activeTab === section.id ? 0 : -1"
            role="tab"
            type="button"
            @click="activeTab = section.id"
          >
            {{ section.label }}
          </button>
        </div>

        <div
          v-for="section in sectionsByMode[activeMode].sections"
          v-show="activeTab === section.id"
          :key="section.id"
          class="tab-panel"
          role="tabpanel"
        >
          <h3>{{ section.title }}</h3>
          <div v-if="section.details.length" class="detail-tree" aria-label="Details">
            <ul class="detail-list">
              <li
                v-for="([term, description], index) in section.details"
                :key="term"
              >
                <span class="detail-branch" aria-hidden="true">
                  {{ index === section.details.length - 1 ? '└─' : '├─' }}
                </span>
                <dl class="detail-entry">
                  <dt>{{ term }}</dt>
                  <dd>{{ description }}</dd>
                </dl>
              </li>
            </ul>
          </div>
          <p>{{ section.body }}</p>
        </div>
        </article>

        <ProfileLinks :links="profileLinks" />
      </div>
    </section>
  </main>
</template>

<style scoped>
.page-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 6rem;
  width: min(147.5rem, calc(100% - 6rem));
  margin: 0 auto;
  padding: 4.5rem 0 5rem;
}

.hero {
  display: grid;
  align-items: center;
  gap: 4rem;
  padding: 1.25rem 0 0.75rem;
}

.hero-portrait {
  display: flex;
}

.hero-portrait-frame {
  width: min(100%, 22.5rem);
}

.hero-copy {
  display: grid;
  gap: 0.5rem;
  align-content: center;
}

.hero-prompt {
  display: flex;
  align-items: baseline;
  gap: 1rem;
  font-size: 2rem;
  user-select: none;
}

.hero-command {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 2rem;
  user-select: none;
}

.section-label {
  text-transform: uppercase;
  letter-spacing: 0.16em;
  color: var(--text-muted);
  font-size: 1.75rem;
}

.prompt-glyph {
  display: inline-block;
  width: 1ch;
  color: var(--prompt-glyph);
}

.prompt-path {
  color: var(--prompt-path);
}

.prompt-connector {
  color: var(--text-muted);
}

.prompt-branch {
  color: var(--prompt-branch);
}

.hero-title {
  font-size: 2rem;
  line-height: 1.1;
  text-transform: lowercase;
}

@media (min-width: 500px) {
  .hero-title {
    font-size: 2.5rem;
  }
}

.content-grid {
  display: grid;
  gap: 4rem;
}

.content-area {
  display: grid;
  gap: 2rem;
}

.content-section {
  display: grid;
  gap: 1.5rem;
  padding: 0;
}

.section-copy {
  display: grid;
  gap: 1.25rem;
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

.detail-tree {
  display: grid;
  gap: 1rem;
  margin-top: 0.75rem;
}

.detail-list {
  display: grid;
  gap: 1rem;
  list-style: none;
  padding: 0;
  margin: 0;
}

.detail-list li {
  display: grid;
  grid-template-columns: 2.5rem minmax(0, 1fr);
  align-items: start;
  column-gap: 0.75rem;
}

.detail-branch {
  color: var(--text-muted);
  width: 2.5rem;
  flex: 0 0 auto;
}

.detail-entry {
  display: grid;
  grid-template-columns: 9.5rem minmax(0, 1fr);
  gap: 3rem;
  margin: 0;
  min-width: 0;
}

.detail-entry dt {
  color: var(--text-muted);
}

.detail-entry dd {
  margin: 0;
  color: var(--text-primary);
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

.content-section :deep(h2),
.tab-panel h3 {
  font-size: 2.5rem;
}

@media (min-width: 500px) {
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

@media (min-width: 800px) {
  .page-shell {
    gap: 5rem;
    width: min(147.5rem, calc(100% - 12rem));
  }

  .content-grid {
    grid-template-columns: minmax(0, 1fr) minmax(26rem, 34rem);
    align-items: start;
  }

  .links-panel {
    align-content: start;
    min-width: 0;
  }
}
</style>
