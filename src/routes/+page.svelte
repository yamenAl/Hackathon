<script>

  import { onMount } from 'svelte';

  // ── Component imports (to be created in subsequent stories) ──
  // import BootSequence     from '$lib/components/BootSequence.svelte';
  import HeroSection      from '$lib/components/HeroSection.svelte';
  // import JourneySection   from '$lib/components/JourneySection.svelte';
  // import SatelliteSection from '$lib/components/SatelliteSection.svelte';
  // import MissionSection   from '$lib/components/MissionSection.svelte';
  // import FooterSection    from '$lib/components/FooterSection.svelte';

  // ── Page state ──

  /** Controls whether the boot sequence has finished */
  let bootComplete = $state(false);

  /** Tracks how far the user has scrolled (0–1) across the full page */
  let scrollProgress = $state(0);

  // ── Lifecycle ──
  onMount(() => {
    /**
     * Listen to scroll events on the window.
     * scrollProgress is a normalized value (0 = top, 1 = bottom)
     * that child sections can use for parallax / reveal effects.
     */
    const handleScroll = () => {
      const scrollTop = window.scrollY;
      const docHeight = document.documentElement.scrollHeight - window.innerHeight;
      scrollProgress  = docHeight > 0 ? scrollTop / docHeight : 0;
    };

    window.addEventListener('scroll', handleScroll, { passive: true });

    // Clean up the listener when the component is destroyed
    return () => window.removeEventListener('scroll', handleScroll);
  });

  /**
   * Called by BootSequence when its animation finishes.
   * Setting bootComplete = true unmounts the boot overlay
   * and reveals the main experience.
   */
  function onBootComplete() {
    bootComplete = true;
  }
</script>

<!-- ============================================================
     SEO / Meta
     (SvelteKit <svelte:head> lets us set per-page <head> tags)
     ============================================================ -->
<svelte:head>
  <title>Nebula Xplorer — Journey into the Black Hole</title>
  <meta
    name="description"
    content="An immersive mission experience aboard the Nebula Xplorer satellite. Scroll into a black hole and discover the science behind the mission."
  />
</svelte:head>

<!-- ============================================================
     Page wrapper
     The `--scroll-progress` custom property is passed down to
     all child sections so they can drive CSS animations from scroll.
     ============================================================ -->
<main
  class="page"
  style="--scroll-progress: {scrollProgress}"
  aria-label="Nebula Xplorer interactive mission experience"
>

  <!-- ── 1. Boot Sequence overlay ──────────────────────────────
       Shown first; sits BELOW the navbar (z-index: 50) so the
       navbar remains visible during the boot animation.
       Hidden once `bootComplete` flips to true.
       --------------------------------------------------------- -->
  {#if !bootComplete}
    <section class="section section--boot" aria-label="Mission boot sequence">
      <!-- <BootSequence on:complete={onBootComplete} /> -->

      <!-- PLACEHOLDER — remove when BootSequence component is ready -->
      <div class="placeholder placeholder--boot">
        <p class="placeholder__label">[ BOOT SEQUENCE ]</p>
        <p class="placeholder__hint">BootSequence component goes here</p>
        <button class="placeholder__skip" onclick={onBootComplete}>
          Skip → Enter Experience
        </button>
      </div>
    </section>
  {/if}

  <!-- ── 2. Hero Section ───────────────────────────────────────
       Full-viewport black hole hero with a scroll-to-begin CTA.
       Visible immediately after boot completes.
       --------------------------------------------------------- -->
  {#if bootComplete}
    <section class="section section--hero" aria-label="Mission hero">
  <HeroSection {scrollProgress} />
</section>

    <!-- ── 3. Journey Section ───────────────────────────────────
         The core scroll-driven experience.
         As the user scrolls, they "fall" deeper into the black
         hole while story copy and visual effects are revealed.
         -------------------------------------------------------- -->
    <section class="section section--journey" aria-label="Black hole journey">
      <!-- <JourneySection {scrollProgress} /> -->

      <!-- PLACEHOLDER -->
      <div class="placeholder placeholder--journey">
        <p class="placeholder__label">[ JOURNEY SECTION ]</p>
        <p class="placeholder__hint">Scroll-driven black hole storytelling goes here</p>
        <p class="placeholder__meta">scrollProgress: {(scrollProgress * 100).toFixed(1)}%</p>
      </div>
    </section>

    <!-- ── 4. Satellite Section ─────────────────────────────────
         Interactive 3D viewer of the Nebula Xplorer satellite.
         Users can click on individual components (solar panels,
         AOCS, comms antenna, etc.) to open info panels.
         -------------------------------------------------------- -->
    <section class="section section--satellite" aria-label="Interactive satellite viewer">
      <!-- <SatelliteSection /> -->

      <!-- PLACEHOLDER -->
      <div class="placeholder placeholder--satellite">
        <p class="placeholder__label">[ SATELLITE SECTION ]</p>
        <p class="placeholder__hint">3D satellite viewer with clickable components goes here</p>
      </div>
    </section>

    <!-- ── 5. Mission Section ───────────────────────────────────
         Static + animated mission information:
         - Key stats (altitude, duration, orbit type …)
         - Onboard systems overview (AOCS, power, comms …)
         - Mission goal explanation cards
         -------------------------------------------------------- -->
    <section class="section section--mission" aria-label="Mission information">
      <!-- <MissionSection /> -->

      <!-- PLACEHOLDER -->
      <div class="placeholder placeholder--mission">
        <p class="placeholder__label">[ MISSION SECTION ]</p>
        <p class="placeholder__hint">Stats, systems, and mission cards go here</p>
      </div>
    </section>

    <!-- ── 6. Footer Section ────────────────────────────────────
         Credits, mission links, and social / contact info.
         -------------------------------------------------------- -->
    <footer class="section section--footer" aria-label="Site footer">
      <!-- <FooterSection /> -->

      <!-- PLACEHOLDER -->
      <div class="placeholder placeholder--footer">
        <p class="placeholder__label">[ FOOTER ]</p>
        <p class="placeholder__hint">Credits and links go here</p>
      </div>
    </footer>
  {/if}

</main>

<!-- ============================================================
     Styles — mobile first
     Base styles target mobile (< 480px).
     Tablet overrides at min-width: 480px.
     Desktop overrides at min-width: 1024px.
     Scoped to this component — child components manage their own.
     ============================================================ -->
<style>
  /* ── CSS custom properties (design tokens) ───────────────── */
  :global(:root) {
    --color-void:        #000000;   /* deep space black          */
    --color-space:       #05020f;   /* near-black with blue hint */
    --color-nebula:      #1a0533;   /* deep purple               */
    --color-glow-purple: #7b2fff;   /* primary neon accent       */
    --color-glow-blue:   #00d4ff;   /* secondary neon accent     */
    --color-text:        #e8e0f5;   /* off-white body text       */
    --color-text-muted:  #6b5e8a;   /* dimmed / secondary text   */

    --font-display: 'Courier New', 'Courier', monospace; /* terminal feel */
    --font-body:    'Courier New', 'Courier', monospace;

    --transition-smooth: 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  }

  /* ── Global resets ───────────────────────────────────────── */
  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin:     0;
    padding:    0;
  }

  :global(html) {
    scroll-behavior: smooth;
    background:      var(--color-void);
  }

  :global(body) {
    background:  var(--color-void);
    color:       var(--color-text);
    font-family: var(--font-body);
    overflow-x:  hidden;
  }

  /* ── Page wrapper ────────────────────────────────────────── */
  .page {
    position:   relative;
    width:      100%;
    min-height: 100vh;
  }

  /* ── Section base ────────────────────────────────────────── */
  .section {
    position: relative;
    width:    100%;
  }

  /* ── Boot section ────────────────────────────────────────────
     z-index: 50 keeps the boot overlay BELOW the navbar
     (navbar uses z-index: 100) so the menu stays visible
     during the boot animation.
  ── */
  .section--boot {
    position:        fixed;
    inset:           0;
    z-index:         50;
    background:      var(--color-void);
    display:         flex;
    align-items:     center;
    justify-content: center;
  }

  /* ── Hero — full viewport, offset below fixed navbar ─────── */
  .section--hero {
    min-height:      100vh;
    display:         flex;
    align-items:     center;
    justify-content: center;
    padding-top:     90px; /* matches --nav-height in Navbar.svelte */
  }

  /* ── Journey — tall section for scroll-driven animations ─── */
  .section--journey {
    min-height:  400vh;
    padding-top: 90px;
  }

  /* ── Satellite viewer ────────────────────────────────────── */
  .section--satellite {
    min-height:  100vh;
    padding-top: 90px;
  }

  /* ── Mission info ────────────────────────────────────────── */
  .section--mission {
    min-height:  100vh;
    padding-top: 90px;
  }

  /* ── Footer ──────────────────────────────────────────────── */
  .section--footer {
    min-height: 20vh;
  }

  /* ════════════════════════════════════════════════════════════
     PLACEHOLDER STYLES (dev-only visual guides)
     Mobile first — smallest values as the base.
  ════════════════════════════════════════════════════════════ */
  .placeholder {
    display:         flex;
    flex-direction:  column;
    align-items:     center;
    justify-content: center;
    gap:             0.75rem;
    width:           100%;
    min-height:      inherit;
    padding:         2rem 1rem;   /* compact on mobile */
    border:          1px dashed var(--color-glow-purple);
    opacity:         0.5;
    text-align:      center;
  }

  /* Smaller text on mobile */
  .placeholder__label {
    font-size:      1rem;
    font-weight:    700;
    color:          var(--color-glow-purple);
    letter-spacing: 0.2em;
    text-transform: uppercase;
  }

  .placeholder__hint {
    font-size: 0.75rem;
    color:     var(--color-text-muted);
  }

  .placeholder__meta {
    font-size:   0.7rem;
    color:       var(--color-glow-blue);
    font-family: monospace;
  }

  .placeholder__skip {
    margin-top:  1rem;
    padding:     0.5rem 1.25rem; /* tighter on mobile */
    background:  transparent;
    border:      1px solid var(--color-glow-purple);
    color:       var(--color-glow-purple);
    font-family: var(--font-display);
    font-size:   0.8rem;
    cursor:      pointer;
    transition:  background var(--transition-smooth),
                 color      var(--transition-smooth);
  }

  .placeholder__skip:hover {
    background: var(--color-glow-purple);
    color:      var(--color-void);
  }

  /* ════════════════════════════════════════════════════════════
     TABLET — min-width: 480px
     More breathing room, slightly larger text.
  ════════════════════════════════════════════════════════════ */
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
      padding:   0.5rem 1.5rem;
    }
  }

  /* ════════════════════════════════════════════════════════════
     DESKTOP — min-width: 1024px
     Full sizes, more padding.
  ════════════════════════════════════════════════════════════ */
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
      padding:   0.6rem 2rem;
    }
  }
</style>