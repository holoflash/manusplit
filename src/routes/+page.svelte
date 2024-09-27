<script lang="ts">
    import Lyrics from "../lib/Lyrics.svelte";
</script>

<main>
    <h1>Welcome to the Home Page</h1>
    <p>
        This is the main homepage content. Below, you can submit and view song
        lyrics.
    </p>

    <Lyrics />
</main>

<style>
    main {
        padding: 20px;
        font-family: sans-serif;
    }

    h1 {
        font-size: 2rem;
    }

    p {
        font-size: 1.2rem;
    }
</style>
