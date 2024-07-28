<script lang="ts">
    import anime from "animejs";
    import {primaryColor} from "../store";

    export let title: string;
    export let description: string;
    export let skills: string[];
    export let image: string;
    export let url: string = "";

    let card: HTMLElement;
    let bg: HTMLElement;

    const handleMouseMove = (e: MouseEvent) => {
        let cardRect = card.getBoundingClientRect();
        let xOffset = ((e.clientX - cardRect.left) / cardRect.width) - 0.5;
        let yOffset = ((e.clientY - cardRect.top) / cardRect.height) - 0.5;

        anime.set(bg, {
            translateX: `${xOffset * 5}%`,
            translateY: `${yOffset * 5}%`,
            scale: 1.05
        });
    }

    const handleHover = (e: MouseEvent) => {
        //Subscribe to mousemove event
        addEventListener("mousemove", handleMouseMove);

        anime({
            targets: bg,
            opacity: 0.5,
            duration: 500,
            easing: "easeOutCubic"
        });
    }

    const handleHoverExit = ()=> {
        //Unsubscribe from mousemove event
        removeEventListener("mousemove", handleMouseMove);

        anime({
            targets: bg,
            opacity: 0,
            duration: 500,
            easing: "easeOutCubic"
        });
    }
</script>

<div class="card"
     style="--primary-color: {$primaryColor};"
>
    <div class="background"
         bind:this={bg}
         style="background-image: url({image})">
    </div>
    <div class="content"
         role="group"
         on:mouseenter={handleHover}
         on:mouseleave={handleHoverExit}
         bind:this={card}
    >
        <div>
            {#if url !== ""}
                <a href={url} target="_blank">
                    <h3>
                        {title}
                    </h3>
                    <svg class="arrow" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><!--!Font Awesome Free 6.6.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.--><path fill="#FFFFFF" d="M214.6 41.4c-12.5-12.5-32.8-12.5-45.3 0l-160 160c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 141.2 160 448c0 17.7 14.3 32 32 32s32-14.3 32-32l0-306.7L329.4 246.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3l-160-160z"/></svg>
                </a>
            {:else}
                <h3>
                    {title}
                </h3>
            {/if}

            <p>
                {description}
            </p>
        </div>
        <div>
            <ul>
                {#each skills as skill}
                    <li>{skill}</li>
                {/each}
            </ul>
        </div>
    </div>
</div>

<style>
    .card {
        width: 100%;
        height: 100%;
        min-height: 300px;
        display: grid;
        overflow: hidden;

        font-family: "Inter", sans-serif;
        color: var(--primary-color);
        clip-path: content-box;
    }
    @media (min-width: 430px) {
        .card {
            min-height: 400px;
        }
    }
    .arrow {
        height: 2em;
        width: 2em;
        rotate: 45deg;
    }
    .content {
        grid-row: 1;
        grid-column: 1;

        z-index: 1;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        padding: 1em;
    }
    .background {
        grid-row: 1;
        grid-column: 1;

        opacity: 0;
        background-size: cover;
        background-position: 50% 50%;
    }
    a {
        display: flex;
        gap: 1em;
        align-items: end;
        color: var(--primary-color);
        text-decoration: none;
    }
    h3 {
        margin: 0;
        font-size: xx-large;
        font-family: "Inter", sans-serif;
        font-optical-sizing: auto;
        font-weight: 900;
        font-style: normal;
    }
    p {
        font-size: medium;
    }
    ul {
        list-style-type: none;
        margin: 0;
    }
    li {
        font-size: small;
        text-align: right;
    }
</style>