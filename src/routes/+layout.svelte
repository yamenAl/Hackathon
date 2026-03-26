<script>
  import '$lib/assets/style/styleguide.css';
  import Navbar from "$lib/components/Navbar.svelte";
  import StarbackBackground from "$lib/components/StarbackBackground.svelte";
  import CursorTrail from '$lib/components/CursorTrail.svelte';

  // Svelte 5 runes syntax — replaces the old `export let` slot pattern
  let { children } = $props();
</script>

<!-- Load Audiowide font from Google Fonts (used in styleguide.css) -->
<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link
    href="https://fonts.googleapis.com/css2?family=Audiowide&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<!--
  No <main> wrapper here — +page.svelte manages its own layout.
  The boot sequence needs position: fixed with no padding offset.
-->
<StarbackBackground />
<!-- Navbar appears on every page because it lives in the root layout -->
<Navbar />
<CursorTrail />
{@render children()}

<style>
  /* ── Global resets ────────────────────────────────────── */
  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
</style>
