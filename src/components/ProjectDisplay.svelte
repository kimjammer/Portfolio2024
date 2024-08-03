<script lang="ts">
    import ProjectCard from "./ProjectCard.svelte";
    import {primaryColor} from "../store";
</script>

<h1 class="inter-base"
    style="--primary-color: {$primaryColor}"
>
    projects
</h1>

<div class="container"
     style="--primary-color: {$primaryColor}"
>
    <div class="row">
        <div class="item big"
             style="border-left: none;"
        >
            <ProjectCard
                    title="Lunaspark (Neuro)"
                    description="AI Vtuber that can hear, see, speak, and remember. And a powerful control panel for moderation and managing the stream."
                    skills={["Python", "SvelteKit", "LLMs"]}
                    image="/images/lunaspark.png"
                    url="https://github.com/kimjammer/Neuro"
            />
        </div>
        <div class="item small"
             style="border-right: none;"
        >
            <ProjectCard
                    title="FRC Scouting"
                    description="Easily collect data in real-time and gain insight into your opponents"
                    skills={["SvelteKit", "MongoDB", "Node.js"]}
                    image="/images/scouting.png"
                    url="https://github.com/kimjammer/Scouting"
            />
        </div>
    </div>
    <div class="row">
        <div class="item small"
             style="border-left: none;"
        >
            <ProjectCard
                    title="FRC Robot Code"
                    description="High reliability, high precision autonomous routines."
                    skills={["Java", "Automation"]}
                    image="/images/robotics.jpg"
            />
        </div>
        <div class="item big"
             style="border-right: none;"
        >
            <ProjectCard
                    title="Purdue IT Ticketing Redesign"
                    description="A fresh reimagination of IT ticketing UI/UX."
                    skills={["Figma", "UI / UX"]}
                    image="/images/purdueTickets.png"
            />
        </div>
    </div>
</div>

<style>
    .container {
        padding: 2em;
    }
    .row {
        display: flex;
        flex-direction: column;
    }
    @media(min-width: 430px) {
        .row {
            flex-direction: row;
        }
    }

    .item{
        box-sizing: border-box;
        border: solid var(--primary-color) 1px;
    }
    @media(min-width: 430px) {
        .big {
            flex: 2;
        }
        .small {
            flex: 1;
        }
    }
    h1 {
        font-size: xxx-large;
        margin-left: 0.5em;
        color: var(--primary-color);
    }
    .inter-base {
        font-family: "Inter", sans-serif;
        font-optical-sizing: auto;
        font-weight: 100;
        font-style: italic;
    }
</style>