<script lang="ts">
    import { onMount } from "svelte";
    import anime from "animejs";
    import {primaryColor} from "../store";

    const runNameAnimation = async () => {
        let index = 1;

        //First name animation
        await anime({
            targets: ".name0",
            translateY: [-100, 0],
            opacity: [0,1],
            duration: 1000,
            endDelay: 2000,
            easing: "easeOutExpo",
        }).finished;

        while (true) {
            let timeline = anime.timeline({
                duration: 1000,
                endDelay: 2000,
                easing: "easeOutExpo",
            });
            timeline.add({
                targets: `.name${index}`,
                translateY: [-100, 0],
                opacity: [0,1],
            }, 0);
            timeline.add({
                targets: `.name${index - 1 >= 0 ? index - 1 : 2}`,
                translateY: [0, 100],
                opacity: [1,0],
            }, 0);
            await timeline.finished;

            index = (index + 1) % 3;
        }
    }

    const runRoleAnimation = async () => {
        let index = 1;

        //First role animation
        await anime({
            targets: ".role0",
            translateY: [-100, 0],
            opacity: [0,1],
            duration: 1000,
            endDelay: 3000,
            easing: "easeOutExpo",
        }).finished;

        while (true) {
            let timeline = anime.timeline({
                duration: 1000,
                endDelay: 2000,
                easing: "easeOutExpo",
            });
            timeline.add({
                targets: `.role${index}`,
                translateY: [-100, 0],
                opacity: [0,1],
            }, 0);
            timeline.add({
                targets: `.role${index - 1 >= 0 ? index - 1 : 3}`,
                translateY: [0, 100],
                opacity: [1,0],
            }, 0);
            await timeline.finished;

            index = (index + 1) % 4;
        }
    }

    onMount(() => {
        //Move all animation targets up
        anime.set(".animate", {
            translateY: -100,
            opacity: 0,
        })

        //Start both animation routines
        runNameAnimation();
        runRoleAnimation();
    });
</script>

<div id="container"
style="--primary-color: {$primaryColor};"
>
    <div id="content">
        <div class="line">
            <h1 class="clip">
                <span class="inter-base">
                    I'm
                </span>
                <span class="name1 animate inter-emphasis">
                    (John Kim)
                </span>
                <span class="name2 animate inter-emphasis">
                    (<span class="noto-sans-kr-emphasis">김주환</span>)
                </span>
                <span class="name0 widest animate inter-emphasis">
                    (KimJammer)
                </span>
            </h1>
        </div>
        <div class="line">
            <h1 class="clip">
                <span class="inter-base">
                    a(n)
                </span>
                <span class="role1 animate inter-emphasis">
                    (Designer)
                </span>
                <span class="role2 animate inter-emphasis">
                    (Student)
                </span>
                <span class="role3 animate inter-emphasis">
                    (Traveler)
                </span>
                <span class="role0 widest animate inter-emphasis">
                    (Software Engineer)
                </span>
            </h1>
        </div>
        <div class="line">
            <h1>
                <span class="inter-base">
                    addicted to learning
                </span>
            </h1>
        </div>
    </div>
</div>

<style>
    #container {
        width: 100%;
        height: 100vh;
        display: flex;
        align-items: center;
    }
    #content {
        width: 100%;
        margin-left: 2em;
    }
    .line {
        color: var(--primary-color);
        font-size: large;
    }
    @media (min-width: 430px) {
        .line {
            font-size: xx-large;
        }
    }
    @media (min-width: 1024px) {
        .line {
            font-size: xxx-large;
        }
    }

    /*Space should be allocated for the widest element so line breaks happen correctly*/
    .widest {
        position: relative !important;
    }
    .animate {
        display: inline-block;
        position: absolute;
    }
    .clip {
        clip-path: padding-box;
    }
    .inter-base {
        font-family: "Inter", sans-serif;
        font-optical-sizing: auto;
        font-weight: 100;
        font-style: normal;
        font-variation-settings: "slnt" -10;
    }
    .inter-emphasis {
        font-family: "Inter", sans-serif;
        font-optical-sizing: auto;
        font-weight: 900;
        font-style: normal;
        font-variation-settings: "slnt" -10;
    }
    .noto-sans-kr-emphasis {
        font-family: "Noto Sans KR", sans-serif;
        font-optical-sizing: auto;
        font-size: 0.9em;
        font-weight: 900;
        font-style: normal;
   }
</style>