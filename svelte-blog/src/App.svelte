<script>
	import Post from './components/Post.svelte';
	import { posts } from './posts/posts.js';
</script>

<main>
	<div id="pground_header">
		<div class="pground_title"> &gt; playground </div>
		<div class="pground_subtitle">from <a href="http://ariellechapin.com">arielle bryn chapin</a></div>
	</div>

	<div class="posts">
		{#if posts.length === 0}
			<p>Sorry, I couldn't load the posts for some reason :(</p>
		{:else}
			{#each posts as post}
				<Post id={post.id} title={post.title} date={post.date} category={post.category} postBody={post.postBody} />
			{/each}
		{/if}
		&#9679;
	</div>
</main>

<style>
	main {
		padding: 40px; 
		width: 100%;
		box-sizing: border-box;
		margin: 0 auto;
		line-height: 1.5rem;
	}

	#pground_header {
		text-align: left;
		
	}

	#pground_header, #pground_header a {
		font-family: "Space Mono", monospace;
	}

	.pground_title {
		font-size: 1.3rem;
	}

	.pground_subtitle {
		font-size: 0.8rem;
	}

	.posts {
		padding: 20px;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>