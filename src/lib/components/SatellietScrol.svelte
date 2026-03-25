<script lang="ts">
  import { base } from "$app/paths";
  import { onMount } from "svelte";
  import StoryArticleCard from "$lib/components/StoryArticleCard.svelte";

  /** Must include SvelteKit `base` when the app is served from a subpath. */
  const MODEL_URL = `${base}/models/satellite/Satelliet3dModel.glb`;

  let ready = $state(false);
  let loadError = $state<string | null>(null);

  onMount(() => {
    void import("@google/model-viewer").then(() => {
      ready = true;
    });
  });

  function onModelError(e: Event) {
    const detail = (e as CustomEvent<{ sourceError?: Error }>).detail;
    loadError =
      detail?.sourceError?.message ?? "Model failed to load (check Network tab).";
  }
</script>

<section class="sat-wrap" aria-label="Satellite 3D model">
  <div class="sat-host">
    {#if ready}
      <div class="sat-model">
        <model-viewer
          src={MODEL_URL}
          alt="Satellite 3D model"
          camera-controls
          disable-zoom
          touch-action="none"
          exposure="1"
          shadow-intensity="0.45"
          shadow-softness="0.65"
          onerror={onModelError}
        ></model-viewer>
      </div>
    {/if}
  </div>

  <StoryArticleCard />

  {#if loadError}
    <p class="sat-err" role="alert">{loadError} — requested: {MODEL_URL}</p>
  {/if}
</section>

<style>
  /* Mobile first; breakpoints align with Navbar (480px / 1024px). */

  .sat-wrap {
    width: 100%;
    margin: 0 0 var(--spacing-md);
  }

  .sat-host {
    width: min(100%, var(--sat-viewer-max-height));
    max-height: var(--sat-viewer-max-height);
    aspect-ratio: 1 / 1;
    min-height: 0;
    margin-inline: auto;
    position: relative;
  }

  .sat-model {
    position: absolute;
    inset: 0;
  }

  .sat-model model-viewer {
    display: block;
    width: 100%;
    height: 100%;
    touch-action: none;
  }

  .sat-err {
    margin-top: var(--spacing-sm);
    padding: 0 var(--spacing-sm);
    color: var(--semantic-error);
    font-size: 0.8125rem;
    line-height: 1.45;
    word-break: break-all;
  }

  @media (min-width: 480px) {
    .sat-wrap {
      margin-bottom: var(--spacing-lg);
    }

    .sat-host {
      aspect-ratio: 4 / 3;
      width: min(100%, calc(var(--sat-viewer-max-height) * 4 / 3));
      max-height: var(--sat-viewer-max-height);
    }

    .sat-err {
      padding-inline: var(--spacing-md);
      font-size: 0.875rem;
    }
  }

  @container page (min-width: var(--container-page-desktop)) {
    .sat-wrap {
      max-width: min(var(--layout-content-max), 100%);
      margin-left: auto;
      margin-right: auto;
      margin-bottom: var(--spacing-xl);
    }

    .sat-err {
      padding-inline: 0;
    }
  }
</style>
