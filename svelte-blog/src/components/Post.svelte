<script>
    export let id;
    export let title;
    export let date;
    export let category;
    export let postBody;
</script>

<style>

.post_title {
    font-size: 2rem;
    font-family: 'Alice', serif;
    text-decoration: underline; 
	font-weight: 600;
    letter-spacing: 0.03em; 
    text-align: center;
}

.post_meta {
    font-size: 0.8rem;
    font-family: "Space Mono", monospace;
    margin: 30px 0 20px 0;
    text-align: center;
}

.post_content {
    position: relative;
    max-width: 650px;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: justify;
    line-height: 1.5em;
    font: 100% "noahregular", Helvetica, sans-serif;
    font-size: 1.3em;
}

.post_content * {
    position: relative;
    width: 100%;
}

.post {
    margin: 40px 0;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

</style>


<div class="post" id={id}>
    <div class="post_title">
        {title}
    </div>
    <div class="post_meta">
        {date} &middot; Arielle Chapin &middot; <i>{category}</i>
    </div>
    <div class="post_content">
        <svelte:component this={postBody} />
    </div>

</div>