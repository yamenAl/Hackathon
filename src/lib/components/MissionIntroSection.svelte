<script>
  /**
   * Smoothly scroll to the Journey section.
   * The target section must have id="journey" in +page.svelte.
   */
  function scrollToJourney() {
    const section = document.getElementById('journey');

    if (section) {
      section.scrollIntoView({
        behavior: 'smooth',
        block: 'start'
      });
    }
  }
</script>

<!-- Mission overview cards -->
<section class="mission-scroll" aria-label="Mission overview cards">
  <!--
    Horizontal scroll track.
    Only one card should be visible at a time.
    Users can scroll/swipe horizontally to reveal the next card.
  -->
  <div class="mission-scroll__track">
    <!-- Card 1 -->
    <article class="mission-card" aria-label="Mission background">
      <h3 class="mission-card__title">What the mission stands for</h3>

      <p class="mission-card__text">
        NEBULA – Xplorer staat voor “Netherlands Educational Satellite for Exploration
        of Binary-Linked Astrophysics – X-ray Observer”.
      </p>

      <p class="mission-card__text">
        Ongeveer vierhonderd studenten helpen SRON, veertien Nederlandse
        onderwijsinstellingen en vele industriële partners om deze ruimtemissie te
        ontwikkelen van begin tot eind, onder leiding van wetenschappers en ingenieurs.
      </p>

      <p class="mission-card__text">
        SRON heeft de leiding over NEBULA – Xplorer, waarmee het bijdraagt aan een nieuwe
        generatie wetenschappers, ontwerpers en technici voor het Nederlandse ruimteonderzoek.
      </p>

    </article>

    <!-- Card 2 -->
    <article class="mission-card" aria-label="Mission goals">
      <h3 class="mission-card__title">Scientific and educational goals</h3>

      <p class="mission-card__text">
        De missie heeft twee doelen. Het wetenschappelijke doel is beter begrijpen hoe
        een zwart gat materiaal afsnoept van een ster die er omheen draait.
      </p>

      <p class="mission-card__text">
        Het educatieve doel is dat studenten van Nederlandse onderwijsinstellingen van
        binnenuit meemaken hoe je een ruimtemissie van begin tot eind opbouwt.
      </p>
    </article>
  </div>
    <!-- CTA button inside the first card -->
      <button class="mission-card__cta" type="button" on:click={scrollToJourney}>
        Start your journey
      </button>
</section>
<style>
  /* ============================================================
     SECTION WRAPPER
     - centered viewport for the horizontal card slider
     - only one card should be visible at a time
     ============================================================ */
  .mission-scroll {
    position: relative;
    z-index: 1;

    width: min(760px, calc(100vw - 2rem));
    margin: 0 auto;
    padding: 4rem 0 5rem;

    overflow: hidden;
  }

  /* ============================================================
     HORIZONTAL TRACK
     - horizontal scroll area
     - one card per snap position
     ============================================================ */
  .mission-scroll__track {
    display: flex;
    gap: 1rem;

    overflow-x: auto;
    overflow-y: hidden;

    scroll-snap-type: x mandatory;
    scroll-behavior: smooth;
    -webkit-overflow-scrolling: touch;

    padding-bottom: 1rem;

    scrollbar-width: thin;
    scrollbar-color: rgba(128, 96, 255, 0.45) transparent;
  }

  .mission-scroll__track::-webkit-scrollbar {
    height: 8px;
  }

  .mission-scroll__track::-webkit-scrollbar-track {
    background: transparent;
  }

  .mission-scroll__track::-webkit-scrollbar-thumb {
    background: rgba(128, 96, 255, 0.45);
    border-radius: 999px;
  }

  /* ============================================================
     CARD
     - exactly one full card visible inside the centered viewport
     ============================================================ */
  .mission-card {
    flex: 0 0 100%;
    width: 100%;

    scroll-snap-align: start;
    scroll-snap-stop: always;

    position: relative;
    overflow: hidden;

    min-height: 420px;
    padding: 1.5rem;

    border: 1px solid rgba(128, 96, 255, 0.22);
    border-radius: 24px;

    background:
      linear-gradient(180deg, rgba(18, 18, 36, 0.88), rgba(11, 10, 26, 0.94));

    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);

    box-shadow:
      0 0 0 1px rgba(255, 255, 255, 0.02) inset,
      0 10px 40px rgba(0, 0, 0, 0.28),
      0 0 24px rgba(128, 96, 255, 0.08);

    transition:
      box-shadow 250ms ease,
      border-color 250ms ease,
      transform 250ms ease;
  }

  /* ============================================================
     TOP GLOW LINE
     ============================================================ */
  .mission-card::before {
    content: '';
    position: absolute;
    inset: 0 0 auto 0;
    height: 2px;

    background: linear-gradient(
      90deg,
      transparent,
      rgba(128, 96, 255, 0.25),
      #8060ff,
      rgba(128, 96, 255, 0.25),
      transparent
    );

    box-shadow: 0 0 14px rgba(128, 96, 255, 0.35);
  }

  .mission-card:hover {
    transform: translateY(-2px);
    border-color: rgba(128, 96, 255, 0.4);
    box-shadow:
      0 0 0 1px rgba(255, 255, 255, 0.03) inset,
      0 14px 50px rgba(0, 0, 0, 0.32),
      0 0 36px rgba(128, 96, 255, 0.14);
  }

  /* ============================================================
     TEXT
     ============================================================ */
  .mission-card__title {
    margin: 0 0 1rem;
    font-family: 'Audiowide', sans-serif;
    font-size: 1rem;
    line-height: 1.3;
    color: #f3efff;
  }

  .mission-card__text {
    margin: 0;
    font-family: 'Courier New', monospace;
    font-size: 1rem;
    line-height: 1.8;
    color: rgba(232, 224, 245, 0.84);
  }

  .mission-card__text + .mission-card__text {
    margin-top: 1rem;
  }
 .mission-card__cta {
  display: flex;
  align-items: center;
  justify-content: center;

  min-width: 240px;
  margin: 2.5rem auto 0;
  padding: 0.75rem 1.6rem;

  border: 1px solid rgba(128, 96, 255, 0.95);
  border-radius: 0;

  background: #8060ff;
  color: #120a2c;

  font-family: 'Courier New', monospace;
  font-size: 0.8rem;
  letter-spacing: 0.08em;
  text-transform: none;

  cursor: pointer;

  box-shadow:
    0 0 10px rgba(128, 96, 255, 0.6),
    0 0 24px rgba(128, 96, 255, 0.35);

  transition:
    transform 200ms ease,
    box-shadow 200ms ease,
    background 200ms ease;
}

.mission-card__cta:hover {
  transform: translateY(-2px);
  background: #8f72ff;
  box-shadow:
    0 0 16px rgba(128, 96, 255, 0.8),
    0 0 36px rgba(128, 96, 255, 0.45);
}

.mission-card__cta:focus-visible {
  outline: 2px solid #ffffff;
  outline-offset: 4px;
}

  /* ============================================================
     TABLET
     ============================================================ */
  @media (min-width: 768px) {
    .mission-scroll {
      width: min(820px, calc(100vw - 4rem));
      padding: 5rem 0 6rem;
    }

    .mission-card {
      min-height: 450px;
      padding: 1.75rem;
    }
  }

  /* ============================================================
     DESKTOP
     ============================================================ */
  @media (min-width: 1024px) {
    .mission-scroll {
      width: min(860px, calc(100vw - 6rem));
    }
  }

  /* ============================================================
     REDUCED MOTION
     ============================================================ */
  @media (prefers-reduced-motion: reduce) {
    .mission-scroll__track {
      scroll-behavior: auto;
    }

    .mission-card {
      transition: none;
    }
  }
</style>