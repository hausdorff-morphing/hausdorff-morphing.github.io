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
    <div>
      From
      <select bind:value={from}>
        <option value={''} disabled selected>&mdash;</option>
        {#each fromData as from}
          <option value={from.toLowerCase()}>{from}</option>
        {/each}
      </select>
    </div>
  {/if}

  <div>
    {#if !single}
      to
    {/if}

    <select bind:value={to}>
      <option value={''} disabled selected>&mdash;</option>
      {#each toData as to}
        <option value={to.toLowerCase()}>{to}</option>
      {/each}
    </select>
  </div>

  <button on:click={choose} disabled={single ? !to : !from || !to}>View &rsaquo;</button>
</div>

<style>
  .MorphSelector {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    gap: 0.5rem;

    font-size: 1.5rem;
  }

  @media screen and (min-width: 600px) {
    .MorphSelector {
      flex-direction: row;
    }
  }

  .MorphSelector > div {
    display: flex;
    gap: 0.5rem;
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
