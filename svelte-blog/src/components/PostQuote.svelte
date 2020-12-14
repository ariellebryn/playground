<script>
    export let author;
    export let title;
    export let url;
    export let publication;
</script>

<style>
    .quote {
        max-width: 500px;
        font-size: 1.1rem;
        line-height: 1.5em;
    }

    .quote_mark {
        font-weight: 800;
        font-size: 1.5em;
        padding: 0 5px;
    }
</style>

<p class="quote">
    <span class="quote_mark">&ldquo;</span>
        <slot></slot>
    <span class="quote_mark">&rdquo;</span>
    <br />
    – <a href={url} target="_blank">{author}, <i>"{title}"</i>, {publication}</a>
</p>