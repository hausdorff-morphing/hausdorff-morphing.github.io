<script>
  import { animals, countries, letters } from '$lib/data'

  import morph from '$lib/morph'

  import MorphSelector from '$lib/components/MorphSelector.svelte'
  import Preview from '$lib/components/Preview.svelte'

  function setMorph({ detail }) {
    let [from, to] = [detail.from, detail.to].sort()

    morph.set({ from, to })
  }
</script>

<svelte:head>
  <title>Hausdorff Morphing</title>
</svelte:head>

<main>
  <h1>Hausdorff Morphing</h1>

  <section>
    <h2>Animals</h2>

    <MorphSelector data={animals} on:choose={setMorph} />
  </section>

  <section>
    <h2>Countries</h2>

    <MorphSelector data={countries} on:choose={setMorph} />
  </section>

  <section>
    <h2>Text</h2>

    <MorphSelector data={letters} single on:choose={setMorph} />
  </section>
</main>

{#if $morph}
  <Preview />
{/if}

<style>
  :global(body) {
    margin: 0;
  }

  :global(*) {
    font-family: 'Raleway', Arial, Helvetica, sans-serif;
  }

  :root {
    --accent: rgb(55, 55, 255);
  }

  main {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 1rem;

    width: 100vw;
    height: 100vh;

    overflow: hidden;

    text-align: center;
  }
</style>
