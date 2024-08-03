<script lang="ts">
    import HeroBackground from "../components/HeroBackground.svelte";
    import HeroText from "../components/HeroText.svelte";
    import ProjectDisplay from "../components/ProjectDisplay.svelte";
    import Footer from "../components/Footer.svelte";
    import LoadingAnimation from "../components/LoadingAnimation.svelte";

    //Wait 500ms then make introAnimationDone true (The curtain will have come down.)
    let introAnimationDone = false;
    setTimeout(() => {
        introAnimationDone = true;
    }, 500);
</script>

<svelte:head>
    <title>KimJammer</title>
    <meta name="description" content="KimJammer's Portfolio and website.">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Noto+Sans+KR:wght@100..900&display=swap" rel="stylesheet">
</svelte:head>

<LoadingAnimation/>
{#if introAnimationDone}
    <div class="background">
        <div>
            <div id="heroText">
                <HeroText/>
            </div>
            <HeroBackground/>
        </div>
        <div>
            <ProjectDisplay/>
        </div>
        <div>
            <Footer/>
        </div>
    </div>
{/if}

<style>
    :global(body) {
        margin: 0;
    }
    #heroText {
        position: absolute;
    }
    .background {
        background-color: #151515;
    }
</style>