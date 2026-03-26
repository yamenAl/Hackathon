<script>
  let {
    articles = [
      {
        title: "Design doc & bus module",
        paragraphs: [
          "NEBULA–Xplorer (SRON). Doc SRON-NEBULA-SP-2025-024 Issue 1.1, 10-07-2025; §4.3 bus.",
          "Diagram: X-ray, batteries, trackers, rack, Cubedrive, RF chain, sensors, SADM, wheels, OBC.",
          "SADM plus panels: dedicated interface; spare mounts for later hardware.",
        ],
      },
      {
        title: "Bus layout strategies",
        paragraphs: [
          "Harness and upgrade space; ECSS-safe inserts; CoG≈CoP, tuned MMOI; mid-plane separates bays.",
          "Reaction wheels hug CoG; batteries up top feeding radiator paths.",
        ],
      },
      {
        title: "External: star tracker & propulsion",
        paragraphs: [
          "Two external trackers: science boresight plus 30° slant; sky stays in view.",
          "Propulsion on CoG-normal interface; external mount saves bus width and mass.",
        ],
      },
      {
        title: "External: RF, Sun, magnetics",
        paragraphs: [
          "S-band 3: Z-, X+ faces Earth in ops; TT&C plus science downlink.",
          "Sun sensors 4: four orthogonal faces; ~360° sun-vector acquisition.",
          "Magnetorquers 5: three external units spaced from batteries and noisy electronics.",
        ],
      },
    ],
  } = $props();
</script>

<section class="story-wrap" aria-label="Story slides">
  <div
    class="story-strip"
    role="region"
    aria-roledescription="carousel"
    aria-label="Story slides"
  >
    {#each articles as item, i (item.title + i)}
      <article
        class="story-slide"
        aria-label="Slide {i + 1} of {articles.length}: {item.title}"
      >
        <div class="glass-card">
          <h3 class="story-line">{item.title}</h3>
          {#each item.paragraphs as text}
            <p class="story-line">{text}</p>
          {/each}
        </div>
      </article>
    {/each}
  </div>
</section>

<style>
  /* Full-width slides, centered square: --story-card-below-size, --glass-* in styleguide.css */

  .story-wrap {
    margin-top: var(--spacing-md);
  }

  .story-strip {
    display: flex;
    width: 100%;
    overflow-x: auto;
    overflow-y: hidden;
    scroll-snap-type: x mandatory;
    scroll-padding-inline: var(--spacing-sm);
    padding-block: 0.25rem var(--spacing-sm) 0;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: thin;
    scrollbar-color: var(--scrollbar-thumb) var(--scrollbar-track);
  }

  .story-strip::-webkit-scrollbar {
    height: 6px;
  }

  .story-strip::-webkit-scrollbar-thumb {
    background: var(--scrollbar-thumb);
    border-radius: var(--radius-full);
  }

  .story-slide {
    flex: 0 0 100%;
    min-width: 0;
    scroll-snap-align: start;
    scroll-snap-stop: always;
    padding-inline: var(--spacing-sm);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .story-slide .glass-card {
    width: var(--story-card-below-size);
    max-width: 100%;
    padding: var(--spacing-md);
    border-radius: var(--radius-md);
    background: var(--glass-fill-below);
    border: 1px solid var(--glass-border-strong);
    box-shadow:
      var(--glass-shadow),
      inset 0 1px 0 var(--glass-inner-highlight);
    backdrop-filter: blur(14px);
    -webkit-backdrop-filter: blur(14px);
    overflow: auto;
  }

  .story-line {
    margin: 0;
    max-width: 100%;
    color: var(--primary-purple-200);
  }

  .story-line + .story-line {
    margin-top: 0.45rem;
  }

  .story-slide .story-line:first-child {
    font-family: var(--display-font);
    font-size: 0.95rem;
    line-height: 1.35;
  }

  .story-slide p.story-line {
    font-size: 0.8125rem;
    line-height: 1.45;
    color: #ffffff;
  }

  @media (prefers-reduced-motion: reduce) {
    .story-strip {
      scroll-snap-type: proximity;
    }
  }
</style>
