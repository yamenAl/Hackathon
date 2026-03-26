<script>
  import { onMount } from 'svelte';

  import HeroSection from '$lib/components/HeroSection.svelte';
  import MissionIntroSection from '$lib/components/MissionIntroSection.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import Loading from '$lib/components/Loading.svelte';
  import SatellietScrol from '$lib/components/SatellietScrol.svelte';

  let bootComplete = $state(false);
  let showLoading = $state(true);
  let scrollProgress = $state(0);

  onMount(() => {
    const handleScroll = () => {
      const scrollTop = window.scrollY;
      const docHeight = document.documentElement.scrollHeight - window.innerHeight;
      scrollProgress = docHeight > 0 ? scrollTop / docHeight : 0;
    };

    window.addEventListener('scroll', handleScroll, { passive: true });
    const loadingTimer = window.setTimeout(() => {
      showLoading = false;
    }, 3000);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.clearTimeout(loadingTimer);
    };
  });

  function enterExperience() {
    const v = document.getElementById('blackhole-hero-video');
    if (v instanceof HTMLVideoElement) {
      v.muted = true;
      void v.play().catch(() => {});
    }
    bootComplete = true;
  }

  /**
   * @param {HTMLButtonElement} node
   */
  function bootSkipButton(node) {
    const handler = () => {
      void enterExperience();
    };
    node.addEventListener('click', handler);
    return {
      destroy() {
        node.removeEventListener('click', handler);
      }
    };
  }
</script>

<svelte:head>
  <title>Nebula Xplorer — Journey into the Black Hole</title>
  <meta
    name="description"
    content="An immersive mission experience aboard the Nebula Xplorer satellite. Scroll into a black hole and discover the science behind the mission."
  />
</svelte:head>

<main
  class="page"
  style="--scroll-progress: {scrollProgress}"
  aria-label="Nebula Xplorer interactive mission experience"
>
  <section class="section section--hero" aria-label="Mission hero">
    <HeroSection {scrollProgress} experienceActive={bootComplete} />
  </section>

  {#if !bootComplete}
    {#if showLoading}
      <Loading />
    {/if}

    <section class="section section--boot" aria-label="Mission boot sequence">
      <div class="placeholder placeholder--boot">
        <p class="placeholder__label">[ BOOT SEQUENCE ]</p>
        <p class="placeholder__hint">BootSequence component goes here</p>
        <button type="button" class="placeholder__skip" use:bootSkipButton>
          Skip → Enter Experience
        </button>
      </div>
    </section>
  {/if}

  {#if bootComplete}
    <div class="section-divider" aria-hidden="true"></div>

    <section class="section section--mission-intro" aria-label="Mission introduction">
      <MissionIntroSection />
    </section>

    <section id="journey" class="section section--journey" aria-label="Black hole journey"></section>

    <section class="section section--satellite" aria-label="Interactive satellite viewer" id="satellite">
      <div class="satellite-content">
        <SatellietScrol />
      </div>
    </section>

    <footer class="section section--footer" aria-label="Site footer" id="contact">
      <Footer />
    </footer>
  {/if}
</main>

<style>
  :global(:root) {
    --color-void: #000000;
    --color-space: #05020f;
    --color-nebula: #1a0533;
    --color-glow-purple: #8060ff;
    --color-glow-blue: #00d4ff;
    --color-text: #e8e0f5;
    --color-text-muted: #6b5e8a;

    --font-display: 'Courier New', 'Courier', monospace;
    --font-body: 'Courier New', 'Courier', monospace;

    --transition-smooth: 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  }

  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  :global(html) {
    scroll-behavior: smooth;
    background: var(--color-void);
  }

  :global(body) {
    background: var(--color-void);
    color: var(--color-text);
    font-family: var(--font-body);
    overflow-x: hidden;
  }

  .page {
    position: relative;
    width: 100%;
    min-height: 100vh;
  }

  .section {
    position: relative;
    width: 100%;
  }

  .section-divider {
    width: 100%;
    height: 2px;
    position: relative;
    z-index: 10;

    background: linear-gradient(
      to right,
      transparent 0%,
      rgba(128, 96, 255, 0.4) 20%,
      #8060ff 50%,
      rgba(128, 96, 255, 0.4) 80%,
      transparent 100%
    );

    box-shadow:
      0 0 10px rgba(128, 96, 255, 0.5),
      0 0 30px rgba(128, 96, 255, 0.3);
  }

  .section--boot {
    position: fixed;
    inset: 0;
    z-index: 50;
    background: var(--color-void);
    display: flex;
    align-items: center;
    justify-content: center;
    pointer-events: auto;
  }

  .section--hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 90px;
  }

  .section--journey {
    min-height: 400vh;
    padding-top: 90px;
  }

  .section--satellite {
    min-height: 100vh;
    padding-top: 90px;
    position: relative;
    container-type: inline-size;
    container-name: page;
  }

  .satellite-content {
    max-width: min(var(--layout-content-max, 72rem), 100vw - 2rem);
    margin-inline: auto;
    padding-inline: var(--spacing-sm, 1rem);
    box-sizing: border-box;
  }

  @media (min-width: 480px) {
    .satellite-content {
      padding-inline: var(--spacing-md, 1.25rem);
    }
  }

  @media (min-width: 1024px) {
    .satellite-content {
      padding-inline: 0;
    }
  }

  .section--mission {
    min-height: 100vh;
    padding-top: 90px;
  }

  .section--mission-scroll {
    position: relative;
    min-height: auto;
    background:
      radial-gradient(circle at top center, rgba(128, 96, 255, 0.08), transparent 40%),
      linear-gradient(180deg, #04040e 0%, #060612 100%);
  }

  .section--footer {
    min-height: 0;
    overflow-x: clip;
  }

  .placeholder {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
    width: 100%;
    min-height: inherit;
    padding: 2rem 1rem;
    border: 1px dashed rgba(128, 96, 255, 0.3);
    text-align: center;
  }

  .placeholder__label {
    font-size: 1rem;
    font-weight: 700;
    color: var(--color-glow-purple);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    text-shadow: 0 0 10px rgba(128, 96, 255, 0.28);
  }

  .placeholder__hint {
    font-size: 0.75rem;
    color: var(--color-text-muted);
  }

  .placeholder__meta {
    font-size: 0.7rem;
    color: var(--color-glow-blue);
    font-family: monospace;
  }

  .placeholder__skip {
    margin-top: 1rem;
    padding: 0.6rem 1.4rem;

    background: rgba(128, 96, 255, 0.08);
    border: 1px solid #8060ff;
    color: #8060ff;

    font-family: var(--font-display);
    font-size: 0.8rem;
    cursor: pointer;

    transition: all 0.3s ease;

    box-shadow:
      0 0 10px rgba(128, 96, 255, 0.4),
      0 0 20px rgba(128, 96, 255, 0.2);
  }

  .placeholder__skip:hover {
    background: #8060ff;
    color: #000;

    box-shadow:
      0 0 20px rgba(128, 96, 255, 0.8),
      0 0 40px rgba(128, 96, 255, 0.4);
  }

  .placeholder__skip:focus-visible {
    outline: 2px solid #8060ff;
    outline-offset: 4px;
  }

  @media (min-width: 480px) {
    .placeholder {
      padding: 3rem 1.5rem;
    }

    .placeholder__label {
      font-size: 1.1rem;
    }

    .placeholder__hint {
      font-size: 0.8rem;
    }

    .placeholder__meta {
      font-size: 0.75rem;
    }

    .placeholder__skip {
      font-size: 0.85rem;
      padding: 0.5rem 1.5rem;
    }
  }

  @media (min-width: 1024px) {
    .placeholder {
      padding: 4rem 2rem;
    }

    .placeholder__label {
      font-size: 1.25rem;
    }

    .placeholder__hint {
      font-size: 0.875rem;
    }

    .placeholder__meta {
      font-size: 0.75rem;
    }

    .placeholder__skip {
      font-size: 0.875rem;
      padding: 0.6rem 2rem;
    }
  }
</style>
