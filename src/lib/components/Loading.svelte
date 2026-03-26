<div class="overlay">
  <div class="loader" role="status" aria-live="polite" aria-label="Loading mission">
    <div class="orbit" aria-hidden="true">
      <div class="satellite"></div>
    </div>
  </div>
</div>

<style>
.overlay {
  position: fixed;
  inset: 0;
  z-index: 999;
  display: grid;
  place-items: center;
  pointer-events: none;
  background: rgba(0, 0, 0, 0.45);
  backdrop-filter: blur(4px);
}

.loader {
  display: grid;
  place-items: center;
}

.orbit {
  --orbit-size: 6.8rem;
  position: relative;
  width: var(--orbit-size);
  height: var(--orbit-size);
  border: 1px solid rgba(255, 255, 255, 0.35);
  border-radius: 50%;
  animation: orbit-spin 3.5s linear infinite;
}

.satellite {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0.9rem;
  height: 0.9rem;
  border-radius: 2px;
  background: #e7f2ff;
  box-shadow: 0 0 12px rgba(173, 219, 255, 0.75);
  transform: translate(calc(var(--orbit-size) / 2 - 50%), -50%);
}

.satellite::before,
.satellite::after {
  content: '';
  position: absolute;
  top: 50%;
  width: 1rem;
  height: 0.36rem;
  border-radius: 2px;
  background: linear-gradient(90deg, #6a83ff 0%, #4dcbf8 100%);
  transform: translateY(-50%);
}

.satellite::before {
  right: calc(100% + 0.14rem);
}

.satellite::after {
  left: calc(100% + 0.14rem);
}

@keyframes orbit-spin {
  to {
    transform: rotate(360deg);
  }
}

@media (prefers-reduced-motion: reduce) {
  .orbit {
    animation: none;
  }
}
</style>
