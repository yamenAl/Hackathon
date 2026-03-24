<script>
// Import SvelteKit's page store to track the current route for active link highlighting
import { page } from '$app/stores';

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

<nav class="navbar" role="navigation" aria-label="Main Navigation">

    <!-- ── LOGO / BRAND AREA ─────────────────────────────────
       Clicking the logo brings the user back to the homepage.
       aria-label gives screen readers a meaningful description.
    -->
      <a class="navbar__brand" href="/" aria-label="Nebula Xplorer — home">


    <!-- Simple SVG orbit icon representing the satellite mission -->
    <svg class="navbar__logo" viewBox="0 0 32 32" fill="none"
         xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
      <!-- Outer orbit ring -->
      <ellipse cx="16" cy="16" rx="13" ry="5"
               stroke="currentColor" stroke-width="1.2" opacity="0.4"/>

    <!-- Inner orbit ring -->
      <ellipse cx="16" cy="16" rx="8" ry="3"
               stroke="currentColor" stroke-width="1" opacity="0.6"/>
      <!-- Central satellite body -->
      <circle cx="16" cy="16" r="3" fill="currentColor"/>
      <!-- Moving satellite dot on the outer ring -->
      <circle cx="29" cy="16" r="1.8" fill="currentColor"/>
    </svg>
            
  <!-- Brand name split into two lines for styling -->
    <span class="navbar__brand-name">
      <span class="navbar__brand-top">NEBULA</span>
      <span class="navbar__brand-bottom">XPLORER</span>
    </span>
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
  