<script setup lang="ts">
import TypedIntro from './components/TypedIntro.vue'

const profileLinks = [
  {
    href: 'https://www.linkedin.com/in/henrivaisanen/',
    label: 'LinkedIn',
    description: 'Professional profile and current experience',
  },
  {
    href: 'https://github.com/henettaja',
    label: 'GitHub',
    description: 'Projects, experiments, and what I am learning next',
  },
]
</script>

<template>
  <main class="page-shell">
    <svg class="visual-filter-defs" aria-hidden="true" focusable="false">
      <filter id="binary-threshold">
        <feColorMatrix type="saturate" values="0" />
        <feComponentTransfer color-interpolation-filters="sRGB">
          <feFuncR type="gamma" amplitude="1.15" exponent="1.1" offset="0" />
          <feFuncG type="gamma" amplitude="1.15" exponent="1.1" offset="0" />
          <feFuncB type="gamma" amplitude="1.15" exponent="1.1" offset="0" />
        </feComponentTransfer>
        <feComponentTransfer color-interpolation-filters="sRGB">
          <feFuncR type="discrete" tableValues="0 1" />
          <feFuncG type="discrete" tableValues="0 1" />
          <feFuncB type="discrete" tableValues="0 1" />
        </feComponentTransfer>
      </filter>
    </svg>

    <section class="hero" aria-labelledby="intro-title">
      <p class="hero-kicker">$ ./henri.codes</p>
      <TypedIntro
        id="intro-title"
        class="hero-title"
        text="Hello, I'm Henri Vaisanen."
      />
      <p class="hero-summary">
        Software developer and consultant in Helsinki, focused on TypeScript,
        clear interfaces, and software that feels good to use.
      </p>
    </section>

    <section class="content-grid" aria-label="Introduction">
      <article class="content-section content-section-wide" aria-labelledby="about-title">
        <div class="section-copy">
          <p class="section-label">About</p>
          <h2 id="about-title">Who I am</h2>
          <p>
            I work as a software developer consultant and enjoy building
            products where the implementation stays understandable and the
            experience feels intentional from the first interaction onward.
          </p>
        </div>

        <figure class="portrait-panel">
          <div class="portrait-frame">
            <img
              class="portrait-image"
              src="./assets/me.jpg"
              alt="Portrait of Henri Vaisanen"
            />
          </div>
          <figcaption>
            Rendered as a high-contrast trace instead of a polished headshot.
          </figcaption>
        </figure>
      </article>

      <article class="content-section" aria-labelledby="work-title">
        <p class="section-label">What I do</p>
        <h2 id="work-title">How I spend my time</h2>
        <p>
          I gravitate toward TypeScript-based stacks, declarative and reactive
          ways of building software, and frontend work where product thinking is
          as important as implementation detail.
        </p>
      </article>

      <article class="content-section" aria-labelledby="approach-title">
        <p class="section-label">How I work</p>
        <h2 id="approach-title">What matters to me</h2>
        <p>
          I care about clarity, accessibility, and UX details that make software
          easier to understand, easier to maintain, and more pleasant for the
          people using it every day.
        </p>
      </article>

      <article class="content-section" aria-labelledby="links-title">
        <p class="section-label">Links</p>
        <h2 id="links-title">Where to find me</h2>
        <p>
          The best places to follow what I build, study, and think about are
          here.
        </p>
        <ul class="link-list">
          <li v-for="link in profileLinks" :key="link.href">
            <a :href="link.href" target="_blank" rel="noreferrer">
              <span>{{ link.label }}</span>
              <span>{{ link.description }}</span>
            </a>
          </li>
        </ul>
      </article>
    </section>
  </main>
</template>

<style scoped>
.page-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 5rem;
  width: min(1120px, calc(100% - 2rem));
  margin: 0 auto;
  padding: 2rem 0 5rem;
}

.hero {
  display: grid;
  align-content: center;
  min-height: 100vh;
  gap: 1.5rem;
  padding: 4rem 0 2rem;
}

.hero-kicker,
.section-label {
  text-transform: uppercase;
  letter-spacing: 0.16em;
}

.hero-title {
  max-width: 12ch;
  font-size: clamp(3.5rem, 10vw, 8rem);
  line-height: 0.9;
}

.hero-summary {
  max-width: 42rem;
  font-size: clamp(1.05rem, 2vw, 1.3rem);
}

.content-grid {
  display: grid;
  gap: 1.25rem;
}

.content-section {
  display: grid;
  gap: 1rem;
  padding: 1.5rem;
  border: 1px solid var(--panel-border);
  background: var(--panel-bg);
  box-shadow: 0 0 0 1px var(--panel-shadow) inset;
}

.content-section-wide {
  gap: 1.5rem;
}

.section-copy {
  display: grid;
  gap: 1rem;
}

.portrait-panel {
  display: grid;
  gap: 0.75rem;
}

.portrait-frame {
  position: relative;
  overflow: hidden;
  border: 1px solid var(--panel-border-strong);
  background:
    linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.12),
      rgba(255, 255, 255, 0) 45%
    ),
    #050505;
}

.portrait-frame::after {
  content: '';
  position: absolute;
  inset: 0;
  background:
    linear-gradient(rgba(255, 255, 255, 0.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.08) 1px, transparent 1px);
  background-size: 8px 8px;
  mix-blend-mode: soft-light;
  opacity: 0.3;
  pointer-events: none;
}

.portrait-image {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
  filter: url(#binary-threshold);
  image-rendering: pixelated;
}

.link-list {
  display: grid;
  gap: 1rem;
  list-style: none;
  padding: 0;
}

.link-list a {
  display: grid;
  gap: 0.2rem;
}

.link-list span:last-child,
.portrait-panel figcaption {
  color: var(--text-muted);
}

.visual-filter-defs {
  position: absolute;
  width: 0;
  height: 0;
}

@media (min-width: 820px) {
  .page-shell {
    gap: 6rem;
  }

  .content-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    align-items: start;
  }

  .content-section-wide {
    grid-column: 1 / -1;
    grid-template-columns: minmax(0, 1.2fr) minmax(280px, 0.8fr);
    align-items: center;
  }
}
</style>
