<script lang="ts">
  import { onMount } from 'svelte';

  let canvasEl: HTMLCanvasElement | undefined;

  onMount(() => {
    if (!canvasEl) return;
    const cleanup = initCursorTrail(canvasEl);
    return cleanup;
  });

  function initCursorTrail(canvas: HTMLCanvasElement) {
    const ctx = canvas.getContext('2d');
    if (!ctx) return () => {};

    let width = window.innerWidth;
    let height = window.innerHeight;
    let animationFrameId = 0;

    class Particle {
      x: number;
      y: number;
      vx: number;
      vy: number;
      radius: number;
      lifespan: number;
      charge: number;
      opacity: number;
      color: { r: number; g: number; b: number };

      constructor(x: number, y: number) {
        this.x = x + random(-6, 6);
        this.y = y + random(-6, 6);

        this.vx = random(-1.5, 1.5);
        this.vy = random(-2.5, 2.5);

        
        this.radius = Math.random() > 0.7
          ? random(10, 20)
          : random(3, 8);

        this.lifespan = random(25, 45);
        this.charge = this.lifespan;
        this.opacity = 1;

        // 🎨 FELLE kleuren (jouw theme)
        this.color = Math.random() > 0.5
          ? { r: 140, g: 60, b: 255 }   // brighter purple
          : { r: 0, g: 220, b: 255 };   // brighter blue
      }

      update() {
        this.charge--;
        this.radius -= 0.12;

        this.x += this.vx;
        this.y += this.vy;

        this.opacity = Math.max(0, this.charge / this.lifespan);
      }

      draw(ctx: CanvasRenderingContext2D) {
        if (this.radius <= 0) return;

        const gradient = ctx.createRadialGradient(
          this.x,
          this.y,
          0,
          this.x,
          this.y,
          this.radius
        );

        
        gradient.addColorStop(
          0,
          `rgba(${this.color.r}, ${this.color.g}, ${this.color.b}, ${this.opacity})`
        );
        gradient.addColorStop(
          0.3,
          `rgba(${this.color.r}, ${this.color.g}, ${this.color.b}, ${this.opacity * 0.6})`
        );
        gradient.addColorStop(
          1,
          `rgba(${this.color.r}, ${this.color.g}, ${this.color.b}, 0)`
        );

        ctx.fillStyle = gradient;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
        ctx.fill();
      }

      isDead() {
        return this.charge <= 0 || this.radius <= 0;
      }
    }

    const particles: Particle[] = [];

    function random(min = 0, max = 1) {
      return Math.random() * (max - min) + min;
    }

    function resizeCanvas() {
      if (!ctx) return;
      
      width = window.innerWidth;
      height = window.innerHeight;

      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      canvas.width = width * dpr;
      canvas.height = height * dpr;
      canvas.style.width = `${width}px`;
      canvas.style.height = `${height}px`;

      ctx.setTransform(1, 0, 0, 1, 0, 0);
      ctx.scale(dpr, dpr);
    }

    // MEER PARTICLES → opvallender
    function spawnBurst(x: number, y: number, count = 6) {
      for (let i = 0; i < count; i++) {
        particles.push(new Particle(x, y));
      }

      if (particles.length > 250) {
        particles.splice(0, particles.length - 250);
      }
    }

    function animate() {
      if (!ctx) return;
      
      ctx.clearRect(0, 0, width, height);
      ctx.globalCompositeOperation = 'lighter';

      for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];
        p.update();
        p.draw(ctx);

        if (p.isDead()) {
          particles.splice(i, 1);
        }
      }

      animationFrameId = requestAnimationFrame(animate);
    }

    function handleMouseMove(e: MouseEvent) {
      spawnBurst(e.clientX, e.clientY, 6);
    }

    function handleTouchMove(e: TouchEvent) {
      const touch = e.touches[0];
      if (!touch) return;
      spawnBurst(touch.clientX, touch.clientY, 6);
    }

    function handleResize() {
      resizeCanvas();
    }

    resizeCanvas();
    animate();

    window.addEventListener('mousemove', handleMouseMove, { passive: true });
    window.addEventListener('touchmove', handleTouchMove, { passive: true });
    window.addEventListener('resize', handleResize);

    return () => {
      cancelAnimationFrame(animationFrameId);
      window.removeEventListener('mousemove', handleMouseMove);
      window.removeEventListener('touchmove', handleTouchMove);
      window.removeEventListener('resize', handleResize);
    };
  }
</script>

<canvas bind:this={canvasEl} class="cursor-trail" aria-hidden="true"></canvas>

<style>
  .cursor-trail {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
  }
</style>