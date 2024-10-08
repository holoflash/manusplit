<script lang="ts">
    import { onMount } from "svelte";

    export let lyricsText: string = "";

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
        const storedStates = localStorage.getItem("checkedStates");
        if (storedStates) {
            checkedStates = JSON.parse(storedStates);
        } else {
            checkedStates = new Array(lyricsContent.length)
                .fill(null)
                .map(() => new Array(4).fill(false));
        }
    }

    function updateCheckedState(lineIndex: number, optionIndex: number) {
        checkedStates[lineIndex][optionIndex] =
            !checkedStates[lineIndex][optionIndex];
        localStorage.setItem("checkedStates", JSON.stringify(checkedStates));
    }

    function toggleDropdown(lineIndex: number) {
        dropdownOpen[lineIndex] = !dropdownOpen[lineIndex];
    }

    onMount(() => {
        lyricsContent = parseText(lyricsText);
        loadCheckedStates();
        dropdownOpen = new Array(lyricsContent.length).fill(false);
    });
</script>

<div>
    {#each lyricsContent as line, lineIndex}
        <div>
            {#if typeof line === "string"}
                <div class="line-container">
                    <button on:click={() => toggleDropdown(lineIndex)}>
                        {line}
                    </button>
                    <div class="options-container">
                        {#each Array(4) as _, optionIndex}
                            {#if checkedStates[lineIndex][optionIndex]}
                                <span
                                    class={`selected-option option-${optionIndex + 1}`}
                                    >{["BAR", "ALT", "MEZ", "SOP"][
                                        optionIndex
                                    ]}</span
                                >
                            {/if}
                        {/each}
                    </div>
                </div>
                {#if dropdownOpen[lineIndex]}
                    <div class="dropdown">
                        {#each Array(4) as _, optionIndex}
                            <label class="option-label">
                                <input
                                    type="checkbox"
                                    checked={checkedStates[lineIndex][
                                        optionIndex
                                    ]}
                                    on:change={() =>
                                        updateCheckedState(
                                            lineIndex,
                                            optionIndex,
                                        )}
                                />
                                {["BAR", "ALT", "MEZ", "SOP"][optionIndex]}
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
        </div>
    {/each}
</div>

<div>
    {#each lyricsContent as line, lineIndex}
        <div>
            {#if typeof line === "string"}
                <div class="line-container">
                    <button on:click={() => toggleDropdown(lineIndex)}>
                        {line}
                    </button>
                    <div class="options-container">
                        {#each Array(4) as _, optionIndex}
                            {#if checkedStates[lineIndex][optionIndex]}
                                <span
                                    class={`selected-option option-${optionIndex + 1}`}
                                    >{["BAR", "ALT", "MEZ", "SOP"][
                                        optionIndex
                                    ]}</span
                                >
                            {/if}
                        {/each}
                    </div>
                </div>
                {#if dropdownOpen[lineIndex]}
                    <div class="dropdown">
                        {#each Array(4) as _, optionIndex}
                            <label class="option-label">
                                <input
                                    type="checkbox"
                                    checked={checkedStates[lineIndex][
                                        optionIndex
                                    ]}
                                    on:change={() => {
                                        updateCheckedState(
                                            lineIndex,
                                            optionIndex,
                                        );
                                    }}
                                />
                                {["BAR", "ALT", "MEZ", "SOP"][optionIndex]}
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
        </div>
    {/each}
</div>

<style>
    div {
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
        align-items: center;
        justify-content: center;
        margin: 10px 0;
    }

    .options-container {
        margin-left: 10px;
    }

    .selected-option {
        margin-left: 5px;
        padding: 3px 6px;
        border-radius: 4px;
        color: #fff;
        font-weight: bold;
    }

    .option-1 {
        background-color: green;
    }

    .option-2 {
        background-color: #36a2eb;
    }

    .option-3 {
        background-color: yellow;
    }

    .option-4 {
        background-color: red;
    }

    .all-option {
        margin-left: 5px;
        padding: 3px 6px;
        border-radius: 4px;
        background-color: #8e44ad;
        color: #fff;
        font-weight: bold;
    }
</style>
