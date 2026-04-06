<script setup lang="ts">
import { ref } from 'vue'
import heroDithered from './assets/hero-portrait.png'
import HeroSection from './components/HeroSection.vue'
import ProfileLinks from './components/ProfileLinks.vue'
import ProfileModeSwitcher from './components/ProfileModeSwitcher.vue'
import ProfileTabs from './components/ProfileTabs.vue'
import TreeList from './components/TreeList.vue'
import TreeListItem from './components/TreeListItem.vue'

type ProfileMode = 'dev' | 'human'

const introRevealMs = 1000
const introRevealDistance = '8rem'
const contentRevealDelayMs = 0
const modeSwitcherRevealDelayMs = introRevealMs + 320

const introRevealDuration = `${introRevealMs}ms`
const contentRevealDelayDuration = `${contentRevealDelayMs}ms`

const activeTab = ref('work')
const activeMode = ref<ProfileMode>('human')
const isIntroReady = ref(false)

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

const profileDetails = [
  ['profile', 'frontend focused full-stack developer'],
  ['focus', 'TypeScript-based stacks'],
  ['location', 'Helsinki'],
] as const

const sectionsByMode = {
  dev: {
    sections: [
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'Modern monorepositories, declarative, reactive and functional paradigms, and projects that aim to leave the world better than they found it make me nod with approval. I enjoy the challenge of figuring out the right technical structure for each scenario, whether that means a local implementation choice or the shape of a larger system. I’ve seen how painful things can get when that is overlooked. I believe product quality and code quality are inseparable.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I love working with teams that make calculated decisions through open discussion, even friendly debate. The best technical direction usually comes from people caring enough to challenge each other’s thinking. I thrive in environments where ambition, curiosity, and technical taste are taken seriously.',
        details: [],
      },
    ],
  },
  human: {
    sections: [
      {
        id: 'work',
        label: './work.txt',
        title: '',
        body: 'I build and maintain production applications in client projects, focusing on maintainable front-end architecture. I’ve contributed across the stack and worked directly with clients to deliver production systems while taking ownership of technical direction. I see technical quality as part of delivering a better product, not as a separate concern. I’m especially drawn to projects that aim to leave the world better than they found it. I’m also regularly thanked for enjoyable collaboration by non-technical clients.',
        details: [],
      },
      {
        id: 'approach',
        label: './approach.txt',
        title: '',
        body: 'I love working with teams that make calculated decisions through open discussion, even friendly debate. It cultivates clarity and trust, which leads to better collaboration and a better product. I thrive in environments where ambition, curiosity, and technical taste are taken seriously.',
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
    kind: 'spotify',
  },
  {
    href: 'https://instagram.com/henettaja',
    label: 'Instagram',
    note: 'offline traces',
    kind: 'instagram',
  },
]
</script>

<template>
  <main class="page-shell">
    <HeroSection
      :portrait-src="heroDithered"
      portrait-alt="Portrait of Henri Vaisanen"
      content-inset="1.4rem"
      @ready="isIntroReady = true"
    />

    <Transition name="intro-reveal">
      <section
        v-if="isIntroReady"
        class="content-area"
        aria-labelledby="info-title"
      >
        <div class="section-copy content-heading">
          <div class="identity-panel" aria-label="Profile header">
            <span class="identity-panel-label">whoami</span>
            <div class="identity-panel-body">
              <div class="heading-row">
                <h2 id="info-title">henri_väisänen</h2>
                <ProfileModeSwitcher
                  :active-mode="activeMode"
                  :mode-options="modeOptions"
                  :reveal-delay-ms="modeSwitcherRevealDelayMs"
                  @update:active-mode="activeMode = $event"
                />
              </div>
              <p class="identity-tagline">
                I write code I can stand behind, for causes I can stand behind.
              </p>
              <div class="identity-output">
                <h3 class="identity-section-title">./summary</h3>
                <div class="identity-detail-tree" aria-label="Profile details">
                  <TreeList>
                    <TreeListItem
                      v-for="([term, description], index) in profileDetails"
                      :key="term"
                      :is-last="index === profileDetails.length - 1"
                    >
                      <dl class="identity-detail-entry">
                        <dt>{{ term }}</dt>
                        <dd>{{ description }}</dd>
                      </dl>
                    </TreeListItem>
                  </TreeList>
                </div>
              </div>
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
    </Transition>
  </main>
</template>

<style scoped>
.page-shell {
  --content-inset: 1.4rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 6rem;
  width: min(147.5rem, calc(100% - 6rem));
  margin: 0 auto;
  padding: 2.5rem 0 4rem;
}

.content-grid {
  display: grid;
  gap: 4rem;
  padding-left: var(--content-inset);
}

.content-area {
  display: grid;
  gap: 5rem;
}

.intro-reveal-enter-active {
  transition:
    opacity v-bind(introRevealDuration) ease-in-out,
    transform v-bind(introRevealDuration) ease-in-out;
  transition-delay: v-bind(contentRevealDelayDuration);
}

.intro-reveal-enter-from,
.intro-reveal-leave-to {
  opacity: 0;
  transform: translateY(v-bind(introRevealDistance));
}

.intro-reveal-enter-to,
.intro-reveal-leave-from {
  opacity: 1;
  transform: translateY(0);
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
  padding: 2rem 1.4rem 1.4rem;
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

.identity-tagline {
  color: var(--text-primary);
}

.identity-detail-tree {
  margin-top: 0.25rem;
}

.identity-output {
  display: grid;
  gap: 0.5rem;
}

.identity-section-title {
  color: var(--text-muted);
  font-size: 1.75rem;
  font-weight: 500;
}

.identity-detail-entry {
  display: grid;
  grid-template-columns: 9.5rem minmax(0, 1fr);
  gap: 3rem;
  margin: 0;
}

.identity-detail-entry dt {
  color: var(--text-muted);
}

.identity-detail-entry dd {
  margin: 0;
  color: var(--text-primary);
}

.identity-panel-label {
  position: absolute;
  top: 0;
  left: 1.2rem;
  transform: translateY(-50%);
  padding: 0 0.75rem;
  background: var(--bg);
  color: var(--prompt-path);
  line-height: 1;
}

.heading-row {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 1rem 2rem;
}

@media (min-width: 800px) {
  .page-shell {
    gap: 5rem;
    width: min(147.5rem, calc(100% - 12rem));
    padding: 4.5rem 0 5rem;
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
