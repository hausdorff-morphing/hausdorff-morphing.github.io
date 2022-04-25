<script>
  import { createEventDispatcher } from 'svelte'

  export let data = []
  export let single = false

  let dispatch = createEventDispatcher()

  let from = ''
  let to = ''

  function choose() {
    dispatch('choose', { from, to })
  }

  $: fromData = data.filter((d) => d !== to)
  $: toData = data.filter((d) => d !== from)
</script>

<div class="MorphSelector">
  {#if !single}
    From
    <select bind:value={from}>
      <option value={''} disabled selected>&mdash;</option>
      {#each fromData as from}
        <option value={from}>{from}</option>
      {/each}
    </select>

    to
  {/if}

  <select bind:value={to}>
    <option value={''} disabled selected>&mdash;</option>
    {#each toData as to}
      <option value={to}>{to}</option>
    {/each}
  </select>

  <button on:click={choose} disabled={single ? !to : !from || !to}>View &rsaquo;</button>
</div>

<style>
  .MorphSelector {
    display: flex;
    align-items: center;
    justify-content: center;

    gap: 1rem;

    font-size: 1.5rem;
  }

  select {
    all: initial;
  }

  select,
  button {
    cursor: pointer;

    color: var(--accent);
    font-family: inherit;
    font-size: inherit;
    /* font-weight: 700; */
    text-align: center;
  }

  option {
    color: initial;
    font-weight: initial;
  }

  button {
    border: none;
    background: none;
  }

  button[disabled] {
    cursor: not-allowed;

    opacity: 0.5;
  }

  button:not([disabled]):hover {
    text-decoration: underline;
  }
</style>
