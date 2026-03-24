<script>
// Import SvelteKit's page store to track the current route for active link highlighting
import { page } from '$app/stores';
import LogoIcon from '$lib/assets/svg/LogoIcon.svelte';

 // Navigation links — placeholder hrefs that map to the main sections.
  // Update these hrefs when the actual pages/sections are created.

  const navLinks = [
    { label: 'Mission',    href: '#mission'   },
    { label: 'Black Hole', href: '#blackhole' },
    { label: 'Satellite',  href: '#satellite' },
    { label: 'Systems',    href: '#systems'   },
    { label: 'Contact',    href: '#contact'   },
  ];

  // Tracks whether the mobile hamburger menu is open or closed.
  let menuOpen = false;

// Toggle the mobile menu on hamburger button click.
function toggleMenu() {
  menuOpen = !menuOpen;
}

// Close the mobile menu when a nav link is clicked.
function closeMenu() {
  menuOpen = false;
}

</script>

<!--
  Navbar component — appears on every page via +layout.svelte.
  role="navigation" + aria-label makes this an accessible landmark
  so screen readers can jump straight to the nav.
-->

<nav class="navbar" aria-label="Main Navigation">
  <!-- ── LOGO / BRAND AREA ─────────────────────────────────
   Clicking the logo brings the user back to the homepage.
   aria-label gives screen readers a meaningful description.
-->
<a class="navbar__brand" href="/" aria-label="Nebula Xplorer — home">

  <!-- Official logo component -->
  <LogoIcon />

</a>

 <!-- ── NAVIGATION LINK LIST ──────────────────────────────
       Hidden on mobile; visible on desktop (≥ 768px via CSS).
       Each link scrolls to the corresponding page section.
  -->
  <ul class="navbar__links" role="list">
    {#each navLinks as link}
      <li>
        <!-- 
          class:active checks if the current page hash matches this link.
          This will highlight the correct link as the user navigates.
        -->
        <a
          class="navbar__link"
          class:navbar__link--active={$page.url.hash === link.href}
          href={link.href}
          on:click={closeMenu}
        >
          {link.label}
        </a>
      </li>
    {/each}
    </ul>

 <!-- ── HAMBURGER BUTTON (mobile only) ───────────────────
       aria-expanded tells screen readers if the menu is open.
       aria-controls links this button to the mobile menu below.
       Only visible on screens smaller than 768px.
  -->
  
 <button
    class="navbar__hamburger"
    class:navbar__hamburger--open={menuOpen}
    on:click={toggleMenu}
    aria-expanded={menuOpen}
    aria-controls="mobile-menu"
    aria-label={menuOpen ? 'Close menu' : 'Open menu'}
  >
    <!-- Three lines that animate into an × when the menu is open -->
    <span class="navbar__bar"></span>
    <span class="navbar__bar"></span>
    <span class="navbar__bar"></span>
  </button>
  
 <!-- ── MOBILE DROPDOWN MENU ──────────────────────────────
       id="mobile-menu" matches aria-controls on the button above.
       aria-hidden removes it from the accessibility tree when closed.
  -->

  <div
    id="mobile-menu"
    class="navbar__mobile"
    class:navbar__mobile--open={menuOpen}
    aria-hidden={!menuOpen}
  >
    <ul class="navbar__mobile-links" role="list">
      {#each navLinks as link}
        <li>
          <a
            class="navbar__mobile-link"
            href={link.href}
            on:click={closeMenu}
            tabindex={menuOpen ? 0 : -1}
          >
            {link.label}
            </a>
        </li>
        {/each}
    </ul>
    </div>
</nav> 
<style>
  /* ── Google Fonts ─────────────────────────────────────────
     Audiowide = futuristic display font used throughout the navbar
  ── */
  @import url('https://fonts.googleapis.com/css2?family=Audiowide&display=swap');

  /* ── Design tokens ────────────────────────────────────────
     All colours, sizes and fonts in one place.
  ── */
  :root {
    --nav-height:    90px;
    --nav-bg:        rgba(4, 6, 20, 0.85);
    --nav-border:    rgba(120, 80, 255, 0.2);
    --color-accent:  #7c5cfc;
    --color-accent2: #38bdf8;
    --color-text:    #dde4f0;
    --color-muted:   #6b7a99;
    --font-main:     'Audiowide', sans-serif; /* used everywhere in the navbar */
  }

  /* ══════════════════════════════════════════
     MOBILE FIRST — base styles are for mobile
     Desktop overrides come at the bottom
  ══════════════════════════════════════════ */

  /* ── Navbar bar ───────────────────────────────────────────
     Fixed to the top, full width, glassmorphism effect.
  ── */
  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;

    display: flex;
    align-items: center;
    height: var(--nav-height);
    padding: 0 1.25rem;
    gap: 1rem;

    background: var(--nav-bg);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--nav-border);
  }

  /* ── Brand / Logo ─────────────────────────────────────────*/
  .navbar__brand {
    display: flex;
    align-items: center;
    text-decoration: none;
    flex-shrink: 0;
  }

.navbar__brand :global(.logo-icon) {
  width: 180px;
  height: auto;
  display: block;
  flex-shrink: 0;
  margin-top: 10px;
}

  /* ── Desktop nav links ────────────────────────────────────
     MOBILE: hidden — links live inside the mobile dropdown.
     DESKTOP: overridden to display:flex at the bottom.
  ── */
  .navbar__links {
    display: none;
    list-style: none;
    margin: 0 0 0 auto;
    padding: 0;
    align-items: center;
    gap: 0.25rem;
  }

  .navbar__link {
    display: block;
    padding: 0.4rem 0.75rem;
    font-family: var(--font-main);
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--color-muted);
    text-decoration: none;
    border-radius: 4px;
    transition: color 160ms ease, background 160ms ease;
  }

  .navbar__link:hover,
  .navbar__link--active {
    color: var(--color-text);
    background: rgba(124, 92, 252, 0.1);
  }

  .navbar__link:focus-visible {
    outline: 2px solid var(--color-accent);
    outline-offset: 3px;
  }

  /* ── Hamburger button ─────────────────────────────────────
     MOBILE: visible by default.
     DESKTOP: overridden to display:none at the bottom.
  ── */
  .navbar__hamburger {
    margin-left: auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 5px;
    width: 38px;
    height: 38px;
    padding: 7px;
    background: none;
    border: none;
    cursor: pointer;
    border-radius: 6px;
    transition: background 160ms;
  }

  .navbar__hamburger:hover {
    background: rgba(124, 92, 252, 0.12);
  }

  .navbar__hamburger:focus-visible {
    outline: 2px solid var(--color-accent);
    outline-offset: 2px;
  }

  .navbar__bar {
    display: block;
    width: 100%;
    height: 1.5px;
    background: var(--color-accent);
    border-radius: 2px;
    transform-origin: center;
    transition: transform 280ms ease, opacity 280ms ease;
  }

  /* Middle bar shorter for visual detail */
  .navbar__bar:nth-child(2) {
    width: 65%;
    align-self: flex-end;
  }

  /* Bars animate into × when menu is open */
  .navbar__hamburger--open .navbar__bar:nth-child(1) {
    transform: translateY(6.5px) rotate(45deg);
  }
  .navbar__hamburger--open .navbar__bar:nth-child(2) {
    opacity: 0;
    transform: scaleX(0);
  }
  .navbar__hamburger--open .navbar__bar:nth-child(3) {
    transform: translateY(-6.5px) rotate(-45deg);
  }

  /* ── Mobile dropdown menu ─────────────────────────────────
     MOBILE: visible (display:block), expands via max-height.
     DESKTOP: overridden to display:none at the bottom.
  ── */
  .navbar__mobile {
    position: fixed;
    top: var(--nav-height);
    left: 0;
    right: 0;
    z-index: 99;
    display: block;

    background: rgba(4, 6, 20, 0.97);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--nav-border);

    max-height: 0;
    overflow: hidden;
    transition: max-height 320ms ease;
  }

  .navbar__mobile--open {
    max-height: 400px;
  }

  .navbar__mobile-links {
    list-style: none;
    margin: 0;
    padding: 0.75rem 1.25rem 1.25rem;
    display: flex;
    flex-direction: column;
  }

  .navbar__mobile-link {
    display: block;
    padding: 0.85rem 0;
    font-family: var(--font-main); /* Audiowide here too */
    font-size: 0.7rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--color-muted);
    text-decoration: none;
    border-bottom: 1px solid rgba(120, 80, 255, 0.08);
    transition: color 160ms, padding-left 160ms;
  }

  .navbar__mobile-link:hover {
    color: var(--color-text);
    padding-left: 0.5rem;
  }

  .navbar__mobile-link:focus-visible {
    outline: 2px solid var(--color-accent);
    outline-offset: 3px;
  }

  /* ══════════════════════════════════════════
     DESKTOP OVERRIDES — min-width: 768px
  ══════════════════════════════════════════ */
  @media (min-width: 768px) {
    /* Show the inline link list */
    .navbar__links {
      display: flex;
    }

    /* Hide the hamburger button */
    .navbar__hamburger {
      display: none;
    }

    /* Hide the mobile dropdown */
    .navbar__mobile {
      display: none;
    }
  }
</style>