<script>
    import leftHand from "$lib/assets/img/left-hand-file.png";
    import rightHand from "$lib/assets/img/right-hand-file.png";
</script>

<section class="scroll-scene" aria-label="Scroll animation with hands">
    <div class="stage">
        <div class="lane lane-left">
            <img src={leftHand} alt="Left hand icon" class="hand hand-left" />
        </div>

        <div class="logo-box">
            <h1>NEBULA</h1>
        </div>

        <div class="lane lane-right">
            <img src={rightHand} alt="Right hand icon" class="hand hand-right" />
        </div>
    </div>
</section>

<p class="footer-info">
    <span>WEEK CHALLENGE 2026</span>
    <span>FDND AGENCY</span>
    <span>GUARDIAN</span>
</p>


<style>
    .scroll-scene {
        min-height: 280vh;
        overflow-x: clip;
        view-timeline-name: --scene;
        view-timeline-axis: block;
    }

    .stage {
        position: sticky;
        top: 0;
        height: 100vh;
        width: 100vw;
        max-width: 100vw;
        padding: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 0.75rem;
    }

    .lane {
        flex: 1;
        display: flex;
        overflow: hidden;
        min-width: 0;
    }

    .lane-left {
        justify-content: flex-end;
    }

    .lane-right {
        justify-content: flex-start;
    }

    .logo-box {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0 1rem;
    }

    .logo-box :global(.logo-icon) {
        width: clamp(120px, 18vw, 260px);
        height: auto;
        display: block;
    }

    .hand {
        width: clamp(520px, 88vw, 1180px);
        height: auto;
        display: block;
    }

    .hand-left {
        padding-top: 15rem;
        transform: translateX(-220vw);
    }

    .hand-right {
        padding-top: 30rem;
        transform: translateX(220vw);
    }

    .footer-info {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        padding: 2rem 1rem 3rem;
        color: var(--neutral-grey-500);
        font-size: 0.92rem;
        line-height: 1.4;
        letter-spacing: 0.1em;
        text-transform: uppercase;
        opacity: 0.95;
        gap: 1rem;
    }

    .footer-info > span {
        flex: 1;
        text-align: center;
    }

    .member-names {
        display: inline-flex;
        gap: 1rem;
        margin-left: 0.35rem;
    }

    /* Scroll-timeline animation for modern browsers */
    @supports (animation-timeline: --scene) {
        .hand-left {
            animation-name: left-to-center;
            animation-duration: 1s;
            animation-timing-function: cubic-bezier(0.22, 1, 0.36, 1);
            animation-fill-mode: both;
            animation-timeline: --scene;
            animation-range: entry 28% cover 50%;
        }

        .hand-right {
            animation-name: right-to-center;
            animation-duration: 1s;
            animation-timing-function: cubic-bezier(0.22, 1, 0.36, 1);
            animation-fill-mode: both;
            animation-timeline: --scene;
            animation-range: entry 28% cover 50%;
        }
    }

    /* Fallback animation for older browsers */
    @supports not (animation-timeline: --scene) {
        .hand-left {
            animation: left-to-center 1.1s ease-out both;
        }

        .hand-right {
            animation: right-to-center 1.1s ease-out both;
        }
    }

    @keyframes left-to-center {
        from {
            transform: translateX(-220vw);
        }
        to {
            transform: translateX(0);
        }
    }

    @keyframes right-to-center {
        from {
            transform: translateX(220vw);
        }
        to {
            transform: translateX(0);
        }
    }

    /* Tablet styles */
    @media (max-width: 1024px) {
        .hand-left{
            padding-top: 10rem;
        }
        .hand-right {
            padding-top: 20rem;
        }

    }

    /* Phone styles */
    @media (max-width: 768px) {
        .hand {
            width: clamp(120px, 60vw, 320px);
        }

        .hand-left{
            padding-top: 3rem;
        }
        .hand-right {
            padding-top: 5rem;
        }

        .footer-info {
            display: grid;
            justify-items: center;
            padding: 1.5rem 0.75rem 2.25rem;
            font-size: 0.82rem;
            letter-spacing: 0.08em;
            gap: 0.35rem;
        }
    }
</style>
