<script>
	import '$lib/assets/style/styleguide.css';
  import Navbar from "$lib/components/Navbar.svelte";


	let { children } = $props();
 // Import the Navbar component
 // $lib is Sveltekit's alias for src/lib.
</script>

<!-- Navbar appears on every page because it lives in the root layout -->
<Navbar />
 
<!--  <main> wraps all page content.
  padding-top: 60px pushes content below the fixed navbar.
-->
<main>

	  <!-- <slot /> renders the current page component here-->
  <slot />

</main>

<style>
  /* ── Global resets ────────────────────────────────────── */
  :global(*, *::before, *::after) {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  
  :global(html) {
	/* Smooth scroll for anchor links like #mission, #satellite */
	scroll-behavior: smooth;
  }
 :global(body) {
    background-color: #04040e;
    color: #dde4f0;
    font-family: 'Audiowide', sans-serif;
    overflow-x: hidden;
  }
 
  main {
    /* Must match --nav-height (60px) so content isn't hidden behind the navbar */
    padding-top: 100px;
    min-height: 100vh;
  }
</style>