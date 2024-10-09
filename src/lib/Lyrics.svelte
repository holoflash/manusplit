<script lang="ts">
    import { onMount } from "svelte";

    export let lyricsText: string = "";
    export let storageKey: string = ""; // New variable for page-specific key

    const options = ["BAR", "ALT", "MEZ", "SOP", "KÖR"];

    let lyricsContent: (string | { tag: string; content: string })[] = [];
    let checkedStates: boolean[][] = [];
    let dropdownOpen: boolean[] = [];

    function parseText(
        text: string,
    ): (string | { tag: string; content: string })[] {
        const lines = text.split("\n");
        const result: (string | { tag: string; content: string })[] = [];

        for (const line of lines) {
            if (line.startsWith("# ")) {
                result.push({ tag: "h1", content: line.substring(2).trim() });
            } else if (line.startsWith("## ")) {
                result.push({ tag: "h2", content: line.substring(3).trim() });
            } else if (line.startsWith("### ")) {
                result.push({ tag: "h3", content: line.substring(4).trim() });
            } else if (line.startsWith("***")) {
                result.push({ tag: "br", content: "" });
            } else if (line.trim() !== "") {
                result.push(line.trim());
            }
        }
        return result;
    }

    function loadCheckedStates() {
        const storedStates = localStorage.getItem(
            `checkedStates_${storageKey}`,
        );
        if (storedStates) {
            checkedStates = JSON.parse(storedStates);
        } else {
            checkedStates = lyricsContent.map(() =>
                Array(options.length).fill(false),
            );
        }
    }

    function updateCheckedState(lineIndex: number, optionIndex: number) {
        checkedStates[lineIndex][optionIndex] =
            !checkedStates[lineIndex][optionIndex];
        localStorage.setItem(
            `checkedStates_${storageKey}`,
            JSON.stringify(checkedStates),
        );
    }

    function toggleDropdown(lineIndex: number) {
        dropdownOpen[lineIndex] = !dropdownOpen[lineIndex];
    }

    onMount(() => {
        lyricsContent = parseText(lyricsText);
        loadCheckedStates();
    });
</script>

<div class="content">
    {#each lyricsContent as line, lineIndex}
        {#if typeof line === "string"}
            <div class="line-container">
                <div class="options-container">
                    {#each options as option, optionIndex}
                        {#if checkedStates[lineIndex][optionIndex]}
                            <span class={`selected-option ${option}`}>
                                {option}
                            </span>
                        {/if}
                    {/each}
                </div>
                <button on:click={() => toggleDropdown(lineIndex)}>
                    {line}
                </button>
            </div>

            {#if dropdownOpen[lineIndex]}
                <div class="dropdown">
                    {#each options as option, optionIndex}
                        <label class="option-label">
                            <input
                                type="checkbox"
                                checked={checkedStates[lineIndex][optionIndex]}
                                on:change={() =>
                                    updateCheckedState(lineIndex, optionIndex)}
                            />
                            {option}
                        </label>
                    {/each}
                </div>
            {/if}
        {:else if line.tag === "h1"}
            <h1>{line.content}</h1>
        {:else if line.tag === "h2"}
            <h2>{line.content}</h2>
        {:else if line.tag === "h3"}
            <h3>{line.content}</h3>
        {:else if line.tag === "br"}
            <br />
        {/if}
    {/each}
</div>

<style>
    .content {
        display: flex;
        flex-direction: column;
        text-align: center;
        font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
            "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
    }

    input {
        margin-left: 5px;
    }

    .dropdown {
        align-items: center;
        margin-top: 5px;
        padding: 5px;
        background-color: #f9f9f9;
    }

    button {
        all: unset;
    }

    button:hover {
        cursor: pointer;
    }

    .line-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        margin-block: 5px;
    }
    .options-container {
        display: flex;
    }

    .selected-option {
        padding: 1px 5px;
        color: #fff;
        font-weight: bold;
    }

    .BAR {
        background-color: green;
    }

    .ALT {
        background-color: #36a2eb;
    }

    .MEZ {
        background-color: yellow;
        color: black;
    }

    .SOP {
        background-color: red;
    }
    .KÖR {
        background-color: black;
    }
</style>
