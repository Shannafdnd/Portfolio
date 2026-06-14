<div class="hero">
    <div class="img-container">
        <img src="/src/lib/assets/hero-img.png" alt="">
    </div>
    <div class="hero-right">
        <h1>Portfolio</h1>
        <h3 class="name"><span class="first-name">Shanna</span> <span class="last-name">v/d Werve</span></h3>
        <ul class="contact-info">
            <li>LI: <a href="https://linkedin.com/in/shanna-vdw" target="_blank">/in/shanna-vdw</a></li>
            <li>LI: <a href="https://instagram.com/@s.vdw_" target="_blank">@s.vdw_</a></li>
            <li>WA: <a href="tel:+31683903955">0683903955</a></li>
            <li>Mail: <a href="mailto:shanna.vanderwerve@gmail.com">shanna.vanderwerve@gmail.com</a></li>
        </ul>
    </div>
</div>

<style>
    .hero {
        display: flex;
        align-items: center;
        gap: 3em;
        margin: 0 auto;
        width: max-content;
        height: calc(100vh - 5em);
    }

    .img-container {
        height: min-content;
        box-shadow: -2em 2em #6C7A61;
        margin-left: 2em;
        margin-bottom: 2em;
    }

    .hero-right {
        display: flex;
        flex-direction: column;
        gap: 2em;
    }

    .name {
        display: flex;
        gap: 2em;
    }

    h1,
    .name span,
    .contact-info,
    .img-container img {
        border: 1px solid black;
    }

    .name span {
        padding: 0.25em 1em;
    }

    .contact-info {
        padding: 1em;
    }

    h1 {
        font-size: 6em;
    }
</style>
