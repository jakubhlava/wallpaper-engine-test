<script lang="ts">
    import { onMount } from 'svelte';
    let time = new Date();
    $: hours = ('00'+time.getHours()).slice(-2);
    $: minutes = ('00'+time.getMinutes()).slice(-2);
    $: seconds = ('00'+time.getSeconds()).slice(-2);
    $: panda = "";
    $: fact = "";
    onMount(() => {
        const interval = setInterval(() => {
            time = new Date();
        }, 100);
        randomize();
        return () => {
            clearInterval(interval);
        }
    });

    function randomize() {
        fetch("https://some-random-api.ml/animal/red_panda")
            .then((response) => response.json())
            .then((data) => {panda = data.image; fact = data.fact});
    }
</script>

<div class="fixed top-5 right-5">
    <div class="bg-sky-500 rounded-2xl shadow-lg p-6 text-7xl font-extrabold text-white flex justify-center items-center
                transform-gpu hover:bg-sky-600 duration-300 select-none">
        {hours}:{minutes}:{seconds}
    </div>
</div>

<div class="flex justify-center items-center h-screen w-screen">
    <div class="rounded-2xl bg-sky-500 shadow-lg max-w-xl flex justify-center items.center flex-col">
        <img src="{panda}" alt="" class="m-6 w-11/12 rounded-2xl shadow-lg">
        <div class="flex justify-between mx-6 mb-6">
            <p class="mx-4 text-white">{fact}</p>
            <button class="h-16 mt-auto rounded-2xl bg-slate-800 hover:bg-slate-700 transform-gpu duration-300 active:scale-95 text-white text-lg font-bold p-4 shadow-lg"
            on:click={randomize}>Random</button>
        </div>
    </div>
</div>
