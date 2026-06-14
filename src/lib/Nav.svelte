<script lang="ts">
    let {
        alwaysSticky = false
    } = $props();

    import { onMount } from "svelte";

    let sticky = $derived(alwaysSticky);
    let container: HTMLElement | null = null;

    // $effect(() => {
    //     if (!alwaysSticky) {
    //         onMount(() => {
    //             window.addEventListener('scroll', () => {
    //                 sticky = (container?.getBoundingClientRect()?.y ?? 0) <= 78;
    //             })
    //         })
    //     }
    // })
</script>

<div class="container" class:sticky={sticky} bind:this={container} class:alwaysSticky={alwaysSticky}>
    <nav>
        <div class="nav-items">
            <ul>
                <li><a href="/">About me</a></li>
                <li><a href="/">Resume</a></li>
                <li><a href="/">Work</a></li>
                <li><a class="btn-main" href="/">Contact me!</a></li>
            </ul>
        </div>
    </nav>
</div>

<style>
    .container {
        margin-top: 1em;
        width: 100vw;
    }

    /* if sticky is true but alwaysSticky is false */
    .sticky:not(.alwaysSticky) {
        margin-bottom: 7em;
    }

    nav {
        background-color: var(--paper-color);
        border-bottom: var(--border);
    }

    .sticky nav {
        position: fixed;
        top: 5em;
        left: 0;
        background-color: var(--background-color);
    }

    /* .nav-items {
        max-width: var(--main-width);
    } */

    .sticky .nav-items {
        max-width: 100vw;
        width: 100vw;
        border-top: none;
        border-bottom: var(--border-light);
    }

    ul {
        width: 100%;
        max-width: var(--main-width);
        height: 100%;

        display: flex;
        align-items: end;
        justify-content: end;
        gap: 3em;

        padding: 1.5em 0;
        margin: 0 auto;
        
        list-style: none;
        font-family: var(--menu-item);
        font-size: 14px;
    }

    a {
        font-family: var(--menu-item);
        font-size: 14px;
        font-weight: bold;
    }

    .main-btn:hover {
        color: white;
        transform: scale(1.4);
    }

    a:hover {
        color: #6C7A61;
        transition: var(--hover);
    }

    @media only screen and (max-width: 860px) {
        .container {
            display: none;
        }
    }
</style>