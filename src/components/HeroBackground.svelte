<script lang="ts">
    import {onMount} from "svelte";
    import * as PIXI from 'pixi.js';
    import anime from "animejs";
    import {
        defineHex,
        Direction,
        Grid,
        line,
        rectangle,
        ring,
        Rotation,
        spiral,
        type Traverser
    } from 'honeycomb-grid';
    import {backgroundColor, secondaryColor} from "../store";

    const hexSize = 70;
    const circleGraphicsContext = new PIXI.GraphicsContext()
        .circle(0, 0, 40)
        .fill("#FFFFFF");

    class CustomTile extends defineHex({ dimensions: hexSize }) {
        //The PIXI shapes that should be at this hex's location
        ownedShapes?: PIXI.Graphics[] = [];
        //The PIXI shapes that will be at this hex's location
        ownedQueue?: PIXI.Graphics[] = [];
    }

    let containerElement : HTMLElement;
    let columns : number;
    let rows : number;

    const initPIXI = async () => {
        const app = new PIXI.Application();

        await app.init({
            antialias: true,
            background: $backgroundColor,
            resizeTo: containerElement
        });

        containerElement.appendChild(app.canvas);

        const container = new PIXI.Container();
        app.stage.addChild(container);

        return container;
    }

    function colorIntToHexString(colorInt: number) {
        return "#" + colorInt.toString(16).padStart(6, '0');
    }
    function randomInt(min: number, max: number) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    function handleHover() {
        anime({
            targets: this,
            tint: [colorIntToHexString(this.tint), "#26285a"],
            duration: 300,
            easing: "easeOutCirc"
        }).finished.then(() => {
            anime({
                targets: this,
                tint: [colorIntToHexString(this.tint), $secondaryColor],
                duration: 300,
                easing: "easeOutCirc"
            });
        });
    }

    function initCircle(hex: CustomTile) {
        let circle = new PIXI.Graphics(circleGraphicsContext);
        circle.tint = 0x272727;
        circle.eventMode = "dynamic";
        circle.on('pointerenter', handleHover);
        circle.x = hex.x;
        circle.y = hex.y;
        hex.ownedShapes!.push(circle);
        return circle;
    }

    const randomLine = () => {
        if (randomInt(0,1) === 0) {
            return line({
                start: {col: randomInt(1, columns - 2), row: 0},
                direction: (randomInt(0,1) === 0 ? Direction.SE : Direction.SW),
                length: Math.max(rows, columns)
            })
        } else {
            return line({
                start: {col: randomInt(1, columns - 2), row: rows - 1},
                direction: (randomInt(0,1) === 0 ? Direction.NE : Direction.NW),
                length: Math.max(rows, columns)
            })
        }
    }

    const randomSpiral = () => {
        return spiral({
            start: {col: randomInt(1, columns - 2), row: randomInt(1, rows - 2)},
            radius: randomInt(1, 4),
            rotation: (randomInt(0,1) === 0 ? Rotation.COUNTERCLOCKWISE : Rotation.CLOCKWISE)
        })
    }

    const randomRing = () => {
        const randomCoords = {col: randomInt(1, columns - 2), row: randomInt(1, rows - 2)};
        return ring({
            start: randomCoords,
            center: {col: randomCoords.col + randomInt(1,3), row: randomCoords.row + randomInt(1,3)},
            rotation: (randomInt(0,1) === 0 ? Rotation.COUNTERCLOCKWISE : Rotation.CLOCKWISE)
        })
    }

    onMount(() => {
        (async () =>
        {
            const viewWidth = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
            const viewHeight = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
            const height = 3 / 2 * hexSize;
            const width = Math.sqrt(3) * hexSize;
            columns = Math.round(viewWidth / width) + 1;
            rows = Math.round(viewHeight / height) + 1;

            let container = await initPIXI();

            const grid = new Grid(CustomTile, rectangle({ width: columns, height: rows }))

            //Render hex grid
            grid.forEach((hex) => {
                container.addChild(initCircle(hex));
            })

            //Run continuous animations
            while (true) {
                const randomChoice = randomInt(0,2);
                if (randomChoice === 0) {
                    await runAnimation(container, grid, randomLine());
                } else if (randomChoice === 1) {
                    await runAnimation(container, grid, randomSpiral());
                } else {
                    await runAnimation(container, grid, randomRing());
                }
                await new Promise(resolve => setTimeout(resolve, 1000));
            }
        })();

        async function runAnimation (container: PIXI.Container, grid: Grid<CustomTile>, traverser: Traverser<CustomTile>) {
            let prevHexs: CustomTile[] = [];
            grid.traverse(traverser).forEach((hex) => {
                //Queue prev hex's shapes to be owned by this hex
                let prevHex = prevHexs.at(-1);
                if (prevHex) {
                    prevHex.ownedShapes!.forEach((shape) => {
                        hex.ownedQueue!.push(shape);
                    });
                    prevHex.ownedShapes = [];
                }
                prevHexs.push(hex);
            });

            //Pull all shapes to their owning hexes
            let animationTimeline = anime.timeline({
                duration: 1000,
                easing: 'easeOutElastic'
            });
            animationTimeline.pause();
            grid.forEach((hex) => {
                //Take ownership of all shapes in the queue
                hex.ownedShapes = hex.ownedShapes!.concat(hex.ownedQueue!);
                hex.ownedQueue = [];

                //If a hex has no shape or has too many shapes, correct it.
                if (hex.ownedShapes!.length > 1) {
                    while (hex.ownedShapes!.length > 1) {
                        let shape = hex.ownedShapes!.shift()!;
                        container.removeChild(shape);
                        shape.destroy();
                    }
                } else if (hex.ownedShapes!.length == 0) {
                    container.addChild(initCircle(hex))
                }

                //Animate to new hex location
                hex.ownedShapes!.forEach((shape) => {
                    animationTimeline.add({
                        targets: shape,
                        x: hex.x,
                        y: hex.y,
                    }, 0);
                });
            })
            animationTimeline.play();
            await animationTimeline.finished;
        }
    })
</script>

<div id="background" bind:this={containerElement}>
</div>

<style>
    #background {
        width: 100%;
        height: 110vh;
        overflow: hidden;
    }
</style>