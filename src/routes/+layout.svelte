<script>
  import '$lib/assets/style/styleguide.css';
  import Navbar from '$lib/components/Navbar.svelte';
  import StarbackBackground from '$lib/components/StarbackBackground.svelte';
  import CursorTrail from '$lib/components/CursorTrail.svelte';

  let { children } = $props();
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link
    href="https://fonts.googleapis.com/css2?family=Audiowide&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<StarbackBackground />
<Navbar />
<CursorTrail />

<div class="app-surface">
  {@render children()}
</div>

<style>
  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  .app-surface {
    position: relative;
    z-index: 1;
    min-height: 100dvh;
  }
</style>
