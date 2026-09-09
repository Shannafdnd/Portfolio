<script lang="ts">
	import { onMount } from "svelte";
	import BlurryCircle from "./BlurryCircle.svelte";

    const N_CIRCLES = 10;
    const JIGGLE = 2; // (px/second)
    const PUSH_MULTIPLIER = 5;
    const SPEED = 0.001; // (px/second/px)
    const RADIUS = 350; // (px)
    const MARGIN = 100; // (px) Minimum distance from edge of screen to center of circle
    const BACKGROUND_COLOR_1 = "#F2F0E4" //light green
    const BACKGROUND_COLOR_2 = "#6C7A61" //beige
    const BACKGROUND_COLOR_3 = "#D8B9FF" //lilac
    const CIRCLE_COLORS = [
        "#A967FE",
        "#6C7A61",
    ].map(hexToRGB)

    function hexToRGB(hex: string) {
        const num = parseInt(hex.substring(1), 16);
        const r = num >> 16;
        const g = (num & 65025) >> 8;
        const b = num & 255;
        return `${r}, ${g}, ${b}`
    }

    let width = 0;
    let height = 0;

    class Vec { // This class implements some basic Vector math
        x: number;
        y: number;

        constructor(x: number, y: number) {
            this.x = $state(x);
            this.y = $state(y);
        }
        
        add(other: Vec) {
            this.x += other.x;
            this.y += other.y;
        }

        plus(other: Vec) {
            return new Vec(this.x + other.x, this.y + other.y)
        }
        
        sub(other: Vec) {
            this.x -= other.x;
            this.y -= other.y;
        }

        minus(other: Vec) {
            return new Vec(this.x - other.x, this.y - other.y)
        }
        
        mul(other: number) {
            this.x *= other;
            this.y *= other;
        }

        times(other: number) {
            return new Vec(this.x * other, this.y * other)
        }

        mag() {
            return Math.sqrt(this.x * this.x + this.y * this.y)
        }

        normalize() {
            const mag = this.mag()
            this.x /= mag;
            this.y /= mag;
        }

        normalized() {
            const mag = this.mag()
            return new Vec(this.x / mag, this.y / mag)
        }

        setMag(newMag: number) {
            const mag = this.mag()
            this.x *= newMag / mag;
            this.y *= newMag / mag;
        }
    }

    class Circle {
        pos: Vec;
        radius: number;
        color: String;
        target: Vec;

        constructor(pos: Vec, radius: number, color: String) {
            this.pos = $state(pos);
            this.radius = radius;
            this.color = color;
            this.target = $state(pos.plus(new Vec(randRange(-JIGGLE, JIGGLE), randRange(-JIGGLE, JIGGLE))))
        }

        isInside(pos: Vec) {
            const dx = (pos.x - this.pos.x)
            const dy = (pos.y - this.pos.y)
            return dx * dx + dy * dy < this.radius * this.radius;
        }

        move(dt: DOMHighResTimeStamp) {
            this.target.x = Math.max(MARGIN, Math.min(width - MARGIN, this.target.x))
            this.target.y = Math.max(MARGIN, Math.min(height - MARGIN, this.target.y))
            this.target.add(new Vec(randRange(-JIGGLE, JIGGLE), randRange(-JIGGLE, JIGGLE)).times(dt))
            this.pos.add(this.target.minus(this.pos).times(dt * SPEED))
        }
    }

    function randRange(min: number, max: number) {
        return Math.random() * (max - min) + min
    }

    let circles: Circle[] = $state([]);
    
    let mousePos = new Vec(0, 0);

    onMount(() => {
        width = document.documentElement.clientWidth;
        height = document.documentElement.clientHeight;
        
        for (let i = 0; i < N_CIRCLES; i++) {
            circles.push(new Circle(new Vec(randRange(0, width), randRange(0, height)), RADIUS, CIRCLE_COLORS[Math.floor(randRange(0, CIRCLE_COLORS.length))]))
        }

        document.body.addEventListener("mousemove", (e) => {
            mousePos.x = e.clientX;
            mousePos.y = e.clientY;
        })

        let prevTime: DOMHighResTimeStamp | null = null;
        function update(timestamp: DOMHighResTimeStamp) {
            const dt = prevTime ? timestamp - prevTime : 0;
            for (const circle of circles) {
                if (circle.isInside(mousePos)) {
                    const diff = circle.pos.minus(mousePos)
                    diff.setMag(circle.radius * PUSH_MULTIPLIER)
                    circle.target = mousePos.plus(diff)
                }
                circle.move(dt)
            }

            prevTime = timestamp;
            requestAnimationFrame(update);
        }
        requestAnimationFrame(update);
    })
</script>

<div class="background">
	{#each circles as circle}
        <BlurryCircle pos={circle.pos} radius={circle.radius} color={circle.color}/>
    {/each}
</div>

<style>
    :root {
        --circle-size: 50px;

        --grad-angle: 180deg;
    }

    .background {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        z-index: -1;
        opacity: 0.6;
        overflow: hidden;
        background: linear-gradient(var(--grad-angle), var(--bg-color-1), var(--bg-color-2), var(--bg-color-3));
    }

</style>