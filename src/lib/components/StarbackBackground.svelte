<script>
  import { onMount } from "svelte";

  /** @type {HTMLDivElement | undefined} */
  let rootEl = $state();
  /** @type {HTMLCanvasElement | undefined} */
  let canvasEl = $state();

  onMount(() => {
    if (!canvasEl || !rootEl) return;

    const canvas = canvasEl;
    const root = rootEl;

    /** @type {any} */
    let starback;

    function syncSize() {
      const w = window.innerWidth;
      const h = window.innerHeight;
      canvas.width = w;
      canvas.height = h;
    }

    syncSize();

    const ready = import("starback").then(({ default: Starback }) => {
      starback = new Starback(canvas, {
        type: "dot",
        quantity: 140,
        direction: 225,
        backgroundColor: ["#04040e", "#0e1118", "#1a1530"],
        randomOpacity: true,
        starColor: "rgba(230, 235, 255, 0.95)",
        speed: [1, 0.5],
        starSize: [0.4, 2],
        width: canvas.width,
        height: canvas.height,
      });
    });

    const onResize = () => {
      syncSize();
      if (starback) {
        starback.config.width = canvas.width;
        starback.config.height = canvas.height;
      }
    };

    window.addEventListener("resize", onResize);

    const ro = new ResizeObserver(() => onResize());
    ro.observe(root);

    return () => {
      window.removeEventListener("resize", onResize);
      ro.disconnect();
      void ready;
    };
  });
</script>

<div class="starback-root" bind:this={rootEl}>
  <canvas class="starback-canvas" bind:this={canvasEl} aria-hidden="true"
  ></canvas>
</div>

<style>
  .starback-root {
    position: fixed;
    inset: 0;
    z-index: 0;
    pointer-events: none;
  }

  .starback-canvas {
    display: block;
    width: 100%;
    height: 100%;
  }
</style>
