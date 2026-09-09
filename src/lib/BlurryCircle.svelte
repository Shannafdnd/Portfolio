<script lang="ts">
    let { pos, radius, color } = $props();
</script>

<div class="circle" style:--radius={`${radius}px`} style:--x={`${pos.x}px`} style:--y={`${pos.y}px`} style:--color={color}></div>

<style>
    .circle {
        position: absolute;
        top: 0;
        left: 0;
        width: calc(var(--radius) * 2);
        height: calc(var(--radius) * 2);
        border-radius: var(--radius);
        z-index: 10;
        translate: calc(var(--x) - var(--radius)) calc(var(--y) - var(--radius));
        background-color: rgba(var(--color), 1);
        background: radial-gradient(circle at center, rgba(var(--color), 0.8), rgba(var(--color), 0) 70%) no-repeat;
        mix-blend-mode: darken;
        opacity: 1;
    }
</style>