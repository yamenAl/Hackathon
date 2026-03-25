<script>
    import leftHand from "$lib/assets/img/left-hand-file.png";
    import rightHand from "$lib/assets/img/right-hand-file.png";
    import logo from "$lib/assets/img/logo.svg";
</script>

<section class="scroll-scene" aria-label="Scroll animation with hands">
    <div class="stage">
        <div class="lane lane-left">
            <img src={leftHand} alt="Left hand icon" class="hand hand-left" />
        </div>

        <div class="logo-box">
            <img src={logo} alt="Project logo" class="logo" />
        </div>

        <div class="lane lane-right">
            <img src={rightHand} alt="Right hand icon" class="hand hand-right" />
        </div>
    </div>
</section>

<style>
    :global(body) {
        background-color: #1A0660;
    }

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

    .logo {
        width: clamp(120px, 18vw, 260px);
        height: auto;
        display: block;
    }

    .hand {
        width: clamp(320px, 68vw, 980px);
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
</style>
