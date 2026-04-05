<script setup lang="ts">
import { ref } from 'vue'
import heroDithered from './assets/hero-portrait.png'
import ProfileLinks from './components/ProfileLinks.vue'
import ProfileModeSwitcher from './components/ProfileModeSwitcher.vue'
import ProfileTabs from './components/ProfileTabs.vue'
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
          ['focus', 'TypeScript-based stacks'],
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
        body: 'I like working with teams that make calculated decisions based on open discussion, even friendly debate. The best technical direction usually comes from people caring enough to challenge each other’s thinking.',
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
          ['focus', 'TypeScript-based stacks'],
          ['location', 'Helsinki'],
        ],
      },
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I build and maintain production applications in client projects, focusing on maintainable front-end architecture. I’ve contributed across the stack and worked directly with clients to deliver production systems while taking ownership of technical direction. I’m especially drawn to projects that aim to leave the world better than they found it. Although this site leans technical, I’m also regularly thanked for enjoyable collaboration by non-technical clients.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I like working with teams that make calculated decisions based on open discussion, even friendly debate. It cultivates clarity and trust, which leads to better collaboration and a better product.',
        details: [],
      },
    ],
  },
} as const

const profileLinks = [
  {
    href: 'https://www.linkedin.com/in/henrivaisanen/',
    label: 'LinkedIn',
    note: 'career',
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
        <div class="identity-panel" aria-label="Profile header">
          <span class="identity-panel-label">whoami</span>
          <div class="identity-panel-body">
            <div class="heading-row">
              <h2 id="info-title">henri_väisänen</h2>
              <ProfileModeSwitcher
                :active-mode="activeMode"
                :mode-options="modeOptions"
                @update:active-mode="activeMode = $event"
              />
            </div>
            <p>I write code I can stand behind, for causes I can stand behind.</p>
          </div>
        </div>
      </div>

      <div class="content-grid" aria-label="Introduction">
        <ProfileTabs
          :active-tab="activeTab"
          :sections="sectionsByMode[activeMode].sections"
          @update:active-tab="activeTab = $event"
        />

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

.identity-panel {
  position: relative;
  display: inline-block;
  justify-self: start;
  width: fit-content;
  max-width: 100%;
  padding: 1.6rem 1.4rem 1.4rem;
  border: 1px solid var(--panel-border-strong);
  border-radius: 1.4rem;
  background: linear-gradient(
    180deg,
    rgba(255, 255, 255, 0.025) 0%,
    rgba(255, 255, 255, 0.01) 100%
  );
}

.identity-panel-body {
  display: grid;
  gap: 1.25rem;
}

.identity-panel-label {
  position: absolute;
  top: 0;
  left: 1.2rem;
  transform: translateY(-50%);
  padding: 0 0.75rem;
  background: var(--bg);
  color: var(--text-muted);
  line-height: 1;
}

.heading-row {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 1rem 2rem;
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
