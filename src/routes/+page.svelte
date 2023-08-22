<svelte:head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@1/css/pico.min.css">
    <link rel="stylesheet" href="popup.css" />
</svelte:head>

<script lang="ts">
    let value = '';
    let result = '';

    function select(this: HTMLInputElement, event: Event) { this.select() }

    import { evaluate } from 'mathjs';
    function calculate() {
        const res = evaluate(value);
        result = res;

        chrome.storage.local.set({ equation: value, result }).then(() => {
            console.log("Value is set to " + result);
        })
    }

    import { onMount } from 'svelte';
    onMount(() => {
        chrome.storage.local.get(["result","equation"]).then((store) => {
            const eq = store["equation"];
            const res = store["result"];
        
            value = eq ? eq : '';
            result = res ? res : 'Result';
        });
    })
</script>

<div class="container">
    <h1>Calculator</h1>
    <form on:submit|preventDefault={calculate}>
      <input type="text" placeholder="1 + 2" bind:value={value} on:click={select} />
    </form>

    <p>{result}</p>
</div>