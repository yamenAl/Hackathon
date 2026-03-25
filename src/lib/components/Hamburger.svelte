<script>
  import { onMount } from 'svelte';

  /** @typedef {{ label: string; href: string }} NavLink */
  /** @type {{ links?: NavLink[] }} */
  let { links } = $props();

  /** @type {HTMLInputElement | undefined} */
  let toggle;
  /** @type {HTMLElement | undefined} */
  let container;

  function closeMenu() {
    if (toggle) toggle.checked = false;
  }

  onMount(() => {
    /** @param {MouseEvent} event */
    function handleOutsideClick(event) {
      if (!toggle?.checked || !container) return;
      if (container.contains(/** @type {Node} */ (event.target))) return;
      closeMenu();
    }

    document.addEventListener('click', handleOutsideClick);

    return () => {
      document.removeEventListener('click', handleOutsideClick);
    };
  });
</script>

<nav class="hamburger-nav" aria-label="Mobile menu" bind:this={container}>
  <input id="menu-toggle" class="hamburger-nav__toggle" type="checkbox" bind:this={toggle} />

  <label for="menu-toggle" class="hamburger-nav__button" aria-label="Toggle menu">
    <span class="hamburger-nav__bar"></span>
    <span class="hamburger-nav__bar"></span>
    <span class="hamburger-nav__bar"></span>
  </label>

  <div class="hamburger-nav__panel">
    <ul class="hamburger-nav__list" role="list">
      {#each links as link}
        <li>
          <a href={link.href} class="hamburger-nav__link" onclick={closeMenu}>{link.label}</a>
        </li>
      {/each}
    </ul>
  </div>
</nav>

<style>
  .hamburger-nav {
    position: relative;
    margin-left: auto;
  }

  .hamburger-nav__toggle {
    position: absolute;
    inline-size: 1px;
    block-size: 1px;
    overflow: hidden;
    clip: rect(0 0 0 0);
    clip-path: inset(50%);
    white-space: nowrap;
  }

  .hamburger-nav__button {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 5px;
    width: 38px;
    height: 38px;
    padding: 7px;
    border-radius: 6px;
    cursor: pointer;
    background: transparent;
    border: 1px solid rgba(124, 92, 252, 0.3);
  }

  .hamburger-nav__bar {
    width: 100%;
    height: 2px;
    background: #7c5cfc;
    border-radius: 2px;
    transform-origin: center;
    transition: transform 260ms ease, opacity 260ms ease;
  }

  .hamburger-nav__bar:nth-child(2) {
    width: 65%;
    align-self: flex-end;
  }

  .hamburger-nav__toggle:checked + .hamburger-nav__button .hamburger-nav__bar:nth-child(1) {
    transform: translateY(7px) rotate(45deg);
  }

  .hamburger-nav__toggle:checked + .hamburger-nav__button .hamburger-nav__bar:nth-child(2) {
    opacity: 0;
    transform: scaleX(0);
  }

  .hamburger-nav__toggle:checked + .hamburger-nav__button .hamburger-nav__bar:nth-child(3) {
    transform: translateY(-7px) rotate(-45deg);
  }

  .hamburger-nav__panel {
    position: fixed;
    top: var(--nav-height);
    left: 0;
    right: 0;
    max-height: 0;
    overflow: hidden;
    border-bottom: 1px solid rgba(124, 92, 252, 0.25);
    background: rgba(4, 6, 20, 0.96);
    backdrop-filter: blur(10px);
    transition: max-height 280ms ease;
  }

  .hamburger-nav__toggle:checked ~ .hamburger-nav__panel {
    max-height: 360px;
  }

  .hamburger-nav__list {
    list-style: none;
    margin: 0;
    padding: 0.5rem 1rem 1rem;
  }

  .hamburger-nav__link {
    display: block;
    text-decoration: none;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-size: 0.72rem;
    padding: 0.85rem 0;
    color: #dde4f0;
    border-bottom: 1px solid rgba(120, 80, 255, 0.14);
    transition: color 160ms ease, padding-left 160ms ease;
  }

  .hamburger-nav__link:hover {
    color: #38bdf8;
    padding-left: 0.4rem;
  }

  .hamburger-nav__link:focus-visible {
    outline: 2px solid #7c5cfc;
    outline-offset: 3px;
  }

  .hamburger-nav__list li:last-child .hamburger-nav__link {
    border-bottom: none;
  }

  @media (min-width: 480px) {
    .hamburger-nav__list {
      padding: 0.75rem 1.5rem 1.25rem;
    }

    .hamburger-nav__link {
      font-size: 0.75rem;
    }
  }

  @media (min-width: 1024px) {
    .hamburger-nav {
      display: none;
    }
  }
</style>
