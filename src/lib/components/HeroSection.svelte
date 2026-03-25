<script>
  // ============================================================
  // ── Props ──
  // scrollProgress is passed down from +page.svelte
  // It drives the black hole scale and opacity effects on scroll
  let { scrollProgress = 0 } = $props();

/**
 * Scale the black hole from 1x to 8x as the user scrolls
 * through the hero section (first 25% of total scroll).
 * Clamped so it stops growing after the hero ends.
 */
const heroProgress = $derived(Math.min(scrollProgress * 4, 1));

/**
 * Black hole scale: grows from 1 → 8 as user scrolls.
 * The dramatic zoom creates the "falling in" feeling.
 */
const blackHoleScale = $derived(1 + heroProgress * 7);

/**
 * Fade out the scroll hint once the user starts scrolling.
 * Fully hidden after 10% scroll progress.
 */
const scrollHintOpacity = $derived(Math.max(0, 1 - heroProgress * 10));

/**
 * Dim the accretion disk glow as the user falls deeper.
 * Creates a sense of passing the event horizon.
 */
const glowOpacity = $derived(Math.max(0.2, 1 - heroProgress * 1.5));
</script>
 
<!-- ============================================================
     Hero wrapper
     position: sticky keeps it in view while the user scrolls
     through the Journey section above it.
     ============================================================ -->
<div class="hero">
 
  <!-- Video background — scroll-driven scale blijft werken -->
<div
  class="blackhole"
  style="transform: scale({blackHoleScale});"
  aria-hidden="true"
>
  <video
    class="hero__video"
    autoplay
    muted
    loop
    playsinline
  >
    <!-- Desktop video (768px en groter) — nog niet beschikbaar -->
    <!-- <source media="(min-width: 768px)" src="/videos/blackhole-desktop.mp4" type="video/mp4" /> -->

    <!-- Mobile video — standaard voor nu -->
    <source src="/videos/blackhole-mobile.mp4" type="video/mp4" />
  </video>
</div>
 
  <!-- ── Star field background ─────────────────────────────────
       Static dots that create the sense of deep space.
       CSS generated via box-shadow on a pseudo-element.
       --------------------------------------------------------- -->
  <div class="hero__stars" aria-hidden="true"></div>
 
  <!-- ── Scroll hint ───────────────────────────────────────────
       Subtle animated indicator at the bottom of the screen.
       Fades out once the user starts scrolling.
       No text on mobile — just the animated line.
       --------------------------------------------------------- -->
  <div class="hero__scroll-hint" style="opacity: {scrollHintOpacity};" aria-hidden="true">
    <span class="hero__scroll-line"></span>
    <span class="hero__scroll-label">scroll</span>
  </div>
 
</div>
 
<style>
  /* ════════════════════════════════════════════════════════════
     MOBILE FIRST — base styles (< 480px)
  ════════════════════════════════════════════════════════════ */
 
  /* ── Hero wrapper ────────────────────────────────────────── */
  .hero {
    position:   relative;
    width:      100%;
    height:     100vh;
    overflow:   hidden;
 
    /* Deep space background */
    background: #000000;
 
    /* Center everything inside */
    display:         flex;
    align-items:     center;
    justify-content: center;
  }
 
  /* ── Star field ──────────────────────────────────────────────
     Hundreds of tiny dots using box-shadow on a 1x1 element.
     Performance-friendly — no JS, no canvas needed.
  ── */
  .hero__stars {
    position: absolute;
    inset:    0;
    z-index:  0;
 
    /* Stars are generated as box-shadows on this tiny element */
    width:  1px;
    height: 1px;
    top:    50%;
    left:   50%;
 
    /* Each shadow = one star: x y blur color */
    box-shadow:
      /* Layer 1 — bright stars */
      120px  -340px 2px 1px rgba(255,255,255,0.9),
      -280px  200px 2px 1px rgba(255,255,255,0.8),
      400px  -180px 2px 1px rgba(255,255,255,0.85),
      -150px -420px 2px 1px rgba(255,255,255,0.9),
      320px   380px 2px 1px rgba(255,255,255,0.7),
      -400px  120px 2px 1px rgba(255,255,255,0.8),
      /* Layer 2 — medium stars */
      60px   -200px 1px 0px rgba(255,255,255,0.6),
      -220px  310px 1px 0px rgba(255,255,255,0.5),
      350px   -80px 1px 0px rgba(255,255,255,0.6),
      -80px  -300px 1px 0px rgba(255,255,255,0.55),
      180px   240px 1px 0px rgba(255,255,255,0.5),
      -360px  -60px 1px 0px rgba(255,255,255,0.6),
      290px   160px 1px 0px rgba(255,255,255,0.5),
      -140px  380px 1px 0px rgba(255,255,255,0.55),
      /* Layer 3 — dim distant stars */
      30px   -140px 0px 0px rgba(255,255,255,0.3),
      -190px  80px  0px 0px rgba(255,255,255,0.25),
      240px  -260px 0px 0px rgba(255,255,255,0.3),
      -320px  200px 0px 0px rgba(255,255,255,0.2),
      140px   320px 0px 0px rgba(255,255,255,0.25),
      -60px  -380px 0px 0px rgba(255,255,255,0.3),
      380px    40px 0px 0px rgba(255,255,255,0.2),
      -240px -140px 0px 0px rgba(255,255,255,0.25),
      100px   -60px 0px 0px rgba(255,255,255,0.2),
      -180px  260px 0px 0px rgba(255,255,255,0.3),
      /* Layer 4 — purple-tinted stars (nebula feel) */
      200px  -300px 1px 0px rgba(180,140,255,0.4),
      -300px  160px 1px 0px rgba(140,180,255,0.35),
      440px   200px 1px 0px rgba(180,140,255,0.4),
      -440px -200px 1px 0px rgba(140,180,255,0.3);
  }
 
  /* ── Black hole container ────────────────────────────────────
     Scale is driven by the scrollProgress prop.
     will-change: transform enables GPU compositing for
     smooth 60fps scroll-driven animation.
  ── */
  .blackhole {
    position:     absolute;
    z-index:      1;
    inset:      0;
  }

    .hero__video {
    position:   absolute;
    inset:      0;
    width:      100%;
    height:     100%;
    object-fit: cover;
    object-position: center center;
  }
 
  /* ── Accretion disk — outer ring ─────────────────────────────
     The glowing ring of superheated matter orbiting the black hole.
     Rotates clockwise slowly.
  ── */
  .blackhole__disk {
    position:      absolute;
    border-radius: 50%;
    border:        2px solid transparent;
  }
 
  .blackhole__disk--outer {
    inset:      -20px;
    background: transparent;
 
    /* Glowing ring effect using conic-gradient */
    border: none;
    background: conic-gradient(
      from 0deg,
      transparent 0%,
      rgba(123, 47, 255, 0.6) 15%,
      rgba(0, 212, 255, 0.8) 30%,
      rgba(255, 180, 80, 0.9) 45%,
      rgba(255, 100, 50, 0.7) 55%,
      rgba(123, 47, 255, 0.5) 70%,
      transparent 85%,
      transparent 100%
    );
 
    /* Flatten into an ellipse to suggest a disk seen at angle */
    transform:       scaleY(0.35) rotate(0deg);
    border-radius:   50%;
 
    /* Mask out the center so only the ring shows */
    mask-image: radial-gradient(
      circle at center,
      transparent 38%,
      black 45%,
      black 55%,
      transparent 62%
    );
    -webkit-mask-image: radial-gradient(
      circle at center,
      transparent 38%,
      black 45%,
      black 55%,
      transparent 62%
    );
 
    animation: diskRotate 12s linear infinite;
  }
 
  .blackhole__disk--inner {
    inset:      10px;
    background: conic-gradient(
      from 180deg,
      transparent 0%,
      rgba(255, 140, 40, 0.8) 20%,
      rgba(255, 200, 80, 1.0) 35%,
      rgba(255, 120, 60, 0.7) 50%,
      transparent 65%,
      transparent 100%
    );
 
    transform:   scaleY(0.3) rotate(0deg);
    border-radius: 50%;
 
    mask-image: radial-gradient(
      circle at center,
      transparent 42%,
      black 48%,
      black 58%,
      transparent 64%
    );
    -webkit-mask-image: radial-gradient(
      circle at center,
      transparent 42%,
      black 48%,
      black 58%,
      transparent 64%
    );
 
    /* Counter-rotate for layered depth effect */
    animation: diskRotateReverse 8s linear infinite;
  }
 
  /* ── Event horizon — the dark core ───────────────────────────
     Pure black circle — the point of no return.
     Slightly larger than 50% to cover the disk center.
  ── */
  .blackhole__core {
    position:      absolute;
    inset:         12%;
    border-radius: 50%;
    background:    #000000;
    z-index:       2;
 
    /* Subtle purple inner glow — photon sphere effect */
    box-shadow:
      inset 0 0 30px rgba(123, 47, 255, 0.3),
      inset 0 0 60px rgba(0, 0, 0, 1),
      0 0 40px rgba(123, 47, 255, 0.15),
      0 0 80px rgba(0, 212, 255, 0.08);
  }
 
  /* ── Scroll hint ──────────────────────────────────────────────
     Positioned at the bottom center.
     Fades out as the user starts scrolling.
  ── */
  .hero__scroll-hint {
    position:        absolute;
    bottom:          2rem;
    left:            50%;
    transform:       translateX(-50%);
    z-index:         10;
 
    display:         flex;
    flex-direction:  column;
    align-items:     center;
    gap:             0.5rem;
 
    /* Smooth fade driven by scrollHintOpacity prop */
    transition:      opacity 0.3s ease;
  }
 
  .hero__scroll-line {
    display:    block;
    width:      1px;
    height:     40px;
    background: rgb(255, 255, 255);
 
    /* Animated line that travels downward */
    animation: scrollLine 1.8s ease-in-out infinite;
    transform-origin: top center;
  }
 
  /* Hide label on smallest screens — line alone is enough */
  .hero__scroll-label {
    display:        none;
    font-family:    'Courier New', monospace;
    font-size:      0.9rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color:          rgb(255, 255, 255);
  }
 
  /* ════════════════════════════════════════════════════════════
     KEYFRAME ANIMATIONS
  ════════════════════════════════════════════════════════════ */
 
  /* Accretion disk clockwise rotation */
  @keyframes diskRotate {
    from { transform: scaleY(0.35) rotate(0deg);   }
    to   { transform: scaleY(0.35) rotate(360deg); }
  }
 
  /* Inner disk counter-clockwise rotation */
  @keyframes diskRotateReverse {
    from { transform: scaleY(0.3) rotate(0deg);    }
    to   { transform: scaleY(0.3) rotate(-360deg); }
  }
 
  /* Lensing rings pulse in and out */
  @keyframes lensPulse {
    0%, 100% { opacity: 0.3; transform: scale(1);    }
    50%       { opacity: 0.8; transform: scale(1.02); }
  }
 
  /* Scroll hint line travels downward then resets */
  @keyframes scrollLine {
    0%   { transform: scaleY(0); opacity: 1;   transform-origin: top;    }
    50%  { transform: scaleY(1); opacity: 1;   transform-origin: top;    }
    51%  { transform: scaleY(1); opacity: 1;   transform-origin: bottom; }
    100% { transform: scaleY(0); opacity: 0.2; transform-origin: bottom; }
  }
 
  /* ── Respect reduced motion preference ───────────────────────
     Users who prefer reduced motion get static visuals.
  ── */
  @media (prefers-reduced-motion: reduce) {
    .blackhole__disk--outer,
    .blackhole__disk--inner { animation: none; }
    .blackhole__lens         { animation: none; }
    .hero__scroll-line       { animation: none; }
  }
 
  /* ════════════════════════════════════════════════════════════
     TABLET — min-width: 480px
  ════════════════════════════════════════════════════════════ */
  @media (min-width: 480px) {
    .hero__video {
    object-position: center 30%;
  }
 
    /* Show scroll label on tablet and up */
    .hero__scroll-label {
      display: block;
    }
 
    .hero__scroll-line {
      height: 48px;
    }
  }
 
  /* ════════════════════════════════════════════════════════════
     DESKTOP — min-width: 1024px
  ════════════════════════════════════════════════════════════ */
  @media (min-width: 1024px) {
 
    .hero__scroll-hint {
      bottom: 3rem;
    }
 
    .hero__scroll-line {
      height: 60px;
    }
 
    .hero__scroll-label {
      font-size: 1rem;
    }

     /* Fix video quality on desktop — contain zodat de volledige
     video zichtbaar is zonder bijsnijden op grote schermen */
    .hero__video {
    object-fit:      cover;
    object-position: center center;     
  }
  }
 
  /* ════════════════════════════════════════════════════════════
     LARGE DESKTOP — min-width: 1440px
  ════════════════════════════════════════════════════════════ */
</style>