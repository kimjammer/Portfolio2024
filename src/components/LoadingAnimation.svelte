<style lang="scss">
	:root{
		--animationDuration: 1s;
	}

	.cropcircle{
		overflow: hidden;
		position: fixed;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 100%;
		top: 0;
		left: 0;

		z-index: 9999;
	}

	.container {
		width: 100vmin;
		height: 100vmin;
	}

	svg {
		width: 100%;
		height: 100%;
	}

	.curtain {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 200vh;
		border-radius: 0 0 20vw 20vw;
		z-index: -1;
		background: var(--secondary-color);
		animation: background-move var(--animationDuration)  forwards;
	}

	@keyframes background-move {
		0% {
			transform: translateY(-200%);
		}
		100% {
			transform: translateY(0%);
		}
	}

	.k-left-fill{
		transform: translateY(100%);
		animation: k-left-fill-move var(--animationDuration) infinite forwards;
		animation-direction: alternate;
	}

	#k-left-clipping-mask{
		animation: k-left-mask-move var(--animationDuration) infinite forwards;
		animation-direction: alternate;
	}

	@keyframes k-left-fill-move {
		20% {
			transform: translateY(100%);
		}
		80% {
			transform: translateY(0%);
		}
		100% {
			transform: translateY(0%);
		}
	}
	@keyframes k-left-mask-move {
		50% {
			transform: translateY(0%);
		}
		80% {
			transform: translateY(-29.5%);
		}
		100% {
			transform: translateY(-29.5%);
		}
	}

	.k-upper-fill{
		transform: translate(58.5%, -60%);
		animation: k-upper-fill-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}
	#k-upper-clipping-mask{
		animation: k-upper-mask-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}

	@keyframes k-upper-fill-move {
		20% {
			transform: translate(58.5%, -60%);
		}
		80% {
			transform: translate(0%, 0%);
		}
		100% {
			transform: translate(0%, 0%);
		}
	}
	@keyframes k-upper-mask-move {
		50% {
			transform: translate(0%, 0%);
		}
		80% {
			transform: translate(-21%, 21.6%);
		}
		100% {
			transform: translate(-21%, 21.6%);
		}
	}

	.k-lower-fill{
		transform: translate(39%, 60%);
		animation: k-lower-fill-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}
	#k-lower-clipping-mask{
		animation: k-lower-mask-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}

	@keyframes k-lower-fill-move {
		20% {
			transform: translate(39%, 60%);
		}
		80% {
			transform: translate(0%, 0%);
		}
		100% {
			transform: translate(0%, 0%);
		}
	}
	@keyframes k-lower-mask-move {
		50% {
			transform: translate(0%, 0%);
		}
		75% {
			transform: translate(-19.2%, -29.4%);
		}
		100% {
			transform: translate(-19.2%, -29.4%);
		}
	}

	.j-upper-fill{
		transform: translateY(-67%);
		animation: j-upper-fill-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}
	#j-upper-clipping-mask{
		animation: j-upper-mask-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}

	@keyframes j-upper-fill-move {
		20% {
			transform: translateY(-67%);
		}
		55% {
			transform: translateY(0%);
		}
		100% {
			transform: translateY(0%);
		}
	}
	@keyframes j-upper-mask-move {
		50% {
			transform: translateY(0%);
		}
		80% {
			transform: translateY(21.5%);
		}
		100% {
			transform: translateY(21.5%);
		}
	}

	.j-lower-fill{
		animation: j-lower-fill-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}

	@keyframes j-lower-fill-move {
		50% {
			transform: rotate(0deg);
		}
		80% {
			transform: rotate(90deg);
		}
		100% {
			transform: rotate(90deg);
		}
	}

	.shadow{
		opacity: 0;
		animation: shadow-move var(--animationDuration) forwards infinite;
		animation-direction: alternate;
	}
	@keyframes shadow-move {
		60% {
			opacity: 0;
		}
		80% {
			opacity: 1;
		}
		100% {
			opacity: 1;
		}
	}
</style>

<script lang="ts">
    import {onMount} from "svelte";
	import {backgroundColor, primaryColor, secondaryColor} from "../store";

	let cropcircle: HTMLElement;
    let container: HTMLElement;
    let pageReady = false;

    //When the page loads, animate the cropcircle and logo shrink
    onMount(() => {
        pageReady = true;
    })

    //Every 1 second, check if page has loaded
    const pageLoadCheck = setInterval(() => {
        //If page has loaded, animate the cropcircle and logo shrink
        if (pageReady) {
            cropcircle.animate([
                {clipPath: "circle(100vmax)"},
                {clipPath: "circle(0vmin)"}
            ], {
                duration: 1000,
                easing: "ease",
                fill: "forwards"
            });
            container.animate([
                {scale: "1"},
                {scale: "0"}
            ], {
                duration: 1000,
                easing: "ease",
                fill: "forwards"
            })
            //Clear interval after it has run once
            clearInterval(pageLoadCheck);
        }
    }, 1000);
</script>

<div class="cropcircle" bind:this={cropcircle}
	 style="--background-color: {$backgroundColor};
			--primary-color: {$primaryColor};
			--secondary-color: {$secondaryColor};"
>

    <div class="curtain">
    </div>

    <div class="container" bind:this={container}>
        <svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 500 500" style="enable-background:new 0 0 500 500;" xml:space="preserve">
			<style type="text/css">
				.background{fill: #00000000;}
				.shadow{fill: #212145; clip-path:url(#shadow-clipping-mask);}
				.white{fill: var(--primary-color);}

				.k-left-fill{clip-path:url(#k-left-clipping-mask);}
				.k-upper-fill{clip-path:url(#k-upper-clipping-mask);}
				.k-lower-fill{clip-path:url(#k-lower-clipping-mask);}
				.j-upper-fill{clip-path:url(#j-upper-clipping-mask);}
				.j-lower-group{clip-path:url(#j-lower-clipping-mask);}
				.j-lower-fill{transform-origin: 280px 330px;}
			</style>
            <path class="background" d="M73.6,0h352.9C467.1,0,500,32.9,500,73.6v352.9c0,40.6-32.9,73.5-73.5,73.5H73.6C32.9,500,0,467.1,0,426.5V73.6
			C0,32.9,32.9,0,73.6,0z"/>
            <g>
				<clipPath id="shadow-clipping-mask">
					<path id="shadow-mask" d="M73.6,0h352.9C467.1,0,500,32.9,500,73.6v352.9c0,40.6-32.9,73.5-73.5,73.5H73.6C32.9,500,0,467.1,0,426.5
					V73.6C0,32.9,32.9,0,73.6,0z"/>
				</clipPath>

                <polygon class="shadow" points="500,222.9 500,500 247,500 99.8,352.8 104.7,347.9 104.7,241 127.5,224.2 132.2,106.4 197.9,172.1
					286.8,106.4 378.2,197.7 383.5,106.4"/>
			</g>

            <g>
				<clipPath id="k-left-clipping-mask">
					<rect class="k-left-mask" x="99.8" y="106.4" width="32.4" height="393.6"/>
				</clipPath>
                <rect class="k-left-fill white" x="99.8" y="106.4" width="32.4" height="393.6"/>
			</g>

            <g>
				<clipPath id="k-upper-clipping-mask">
					<polygon class="k-upper-mask" points="392,-1.4 392,1.4 195.3,203.2 173.8,226.3 131.5,268.3 131.5,227.7 356.6,-1.4 "/>
				</clipPath>
                <polygon class="k-upper-fill white" points="392,-1.4 392,1.4 195.3,203.2 173.8,226.3 131.5,268.3 131.5,227.7 356.6,-1.4 "/>
			</g>

            <g>
				<clipPath id="k-lower-clipping-mask">
					<polygon class="k-lower-mask" points="386.6,497.2 386.6,500 351.6,500 173.8,225 195.3,202 "/>
				</clipPath>
                <polygon class="k-lower-fill white" points="386.6,497.2 386.6,500 351.6,500 173.8,225 195.3,202 "/>
			</g>

            <g>
				<clipPath id="j-upper-clipping-mask">
					<rect class="j-upper-mask" x="350.6" y="-1.4" width="32.8" height="333.9"/>
				</clipPath>
                <rect class="j-upper-fill white" x="350.6" y="-1.4" width="32.8" height="333.9"/>
			</g>

            <g>
				<clipPath id="j-lower-clipping-mask">
					<path d="M383.5,331.8v4.8c0,28.6-17.9,60-44.9,72.7c-10.9,5.2-24.1,6.9-36.1,6.9c-6.1,0.1-12.2-1-17.9-3.1l5.8-24.5 c10.2,2.8,26.3,0.3,37.9-4.8c18.2-9,22.2-32.7,22.2-52H383.5z"/>
				</clipPath>
                <g class="j-lower-group">
					<rect class="j-lower-fill white" x="280" y="130" width="200" height="200"/>
				</g>
			</g>
		</svg>
    </div>
</div>
