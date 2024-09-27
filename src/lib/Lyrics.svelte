<script lang="ts">
    import { onMount } from "svelte";

    interface LyricLine {
        line: string;
        index: number;
    }

    let lyricsText: string = "";
    let lyricsArray: LyricLine[] = [];

    function handleSubmit() {
        const lines = lyricsText
            .split("\n")
            .map((line, index) => ({ line, index }));
        lyricsArray = lines;

        localStorage.setItem("lyrics", JSON.stringify(lyricsArray));
    }

    onMount(() => {
        const savedLyrics = localStorage.getItem("lyrics");
        if (savedLyrics) {
            lyricsArray = JSON.parse(savedLyrics);
        }
    });
</script>

<form on:submit|preventDefault={handleSubmit}>
    <textarea
        bind:value={lyricsText}
        rows="10"
        cols="50"
        placeholder="Paste your lyrics here..."
    ></textarea>
    <button type="submit">Submit</button>
</form>

<ul>
    {#each lyricsArray as { line, index }}
        <li>{index + 1}. {line}</li>
    {/each}
</ul>

<style>
    form {
        margin-bottom: 20px;
    }

    textarea {
        width: 100%;
        font-size: 1rem;
    }

    ul {
        list-style: none;
        padding: 0;
    }

    li {
        margin: 5px 0;
    }
</style>
