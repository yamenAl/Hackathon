<script>
  import { page } from '$app/stores';
  import LogoIcon from '$lib/assets/svg/LogoIcon.svelte';
  import Hamburger from '$lib/components/Hamburger.svelte';

  const navLinks = [
    { label: 'Mission', href: '#mission' },
    { label: 'Black Hole', href: '#blackhole' },
    { label: 'Satellite', href: '#satellite' },
    { label: 'Systems', href: '#systems' },
    { label: 'Contact', href: '#contact' }
  ];
</script>

<nav class="navbar" aria-label="Main Navigation">
  <a class="navbar__brand" href="/" aria-label="Nebula Xplorer — home">
    <LogoIcon />
  </a>

  <ul class="navbar__links" role="list">
    {#each navLinks as link}
      <li>
        <a
          class="navbar__link"
          class:navbar__link--active={$page.url.hash === link.href}
          href={link.href}
        >
          {link.label}
        </a>
      </li>
    {/each}
  </ul>

  <Hamburger links={navLinks} />
</nav>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Audiowide&display=swap');

  :root {
    --nav-height: 90px;
    --nav-bg: rgba(4, 6, 20, 0.85);
    --nav-border: rgba(120, 80, 255, 0.2);
    --color-accent: #7c5cfc;
    --color-text: #dde4f0;
    --color-muted: #6b7a99;
    --font-main: 'Audiowide', sans-serif;
  }

  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    height: var(--nav-height);
    padding: 0 1rem;
    gap: 0.75rem;
    background: var(--nav-bg);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--nav-border);
  }

  .navbar__brand {
    display: flex;
    align-items: center;
    text-decoration: none;
    flex-shrink: 0;
  }

  .navbar__brand :global(.logo-icon) {
    width: 140px;
    height: auto;
    display: block;
    flex-shrink: 0;
    margin-top: 10px;
  }

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

  @media (min-width: 480px) {
    .navbar {
      padding: 0 1.5rem;
    }

    .navbar__brand :global(.logo-icon) {
      width: 160px;
    }
  }

  @media (min-width: 1024px) {
    .navbar {
      padding: 0 2rem;
    }

    .navbar__brand :global(.logo-icon) {
      width: 180px;
    }

    .navbar__links {
      display: flex;
    }
  }
</style>
