<script>
  import { onMount } from 'svelte'
  import { fade } from 'svelte/transition'

  import morph from '$lib/morph'

  let loaded = false

  let dilationSrc
  let voronoiSrc
  let mixedSrc

  onMount(async () => {
    let path = `/images/${$morph.from}-${$morph.to}`

    if (!$morph.from) {
      path = `/images/${$morph.to}`
    }

    const paths = [`${path}/dilation.gif`, `${path}/voronoi.gif`, `${path}/mixed.gif`]

    const images = paths.map(
      (path) =>
        new Promise((resolve, reject) => {
          const img = new Image()

          img.onload = function () {
            resolve(path)
          }

          img.onerror = function (ev) {
            reject(path)
          }

          img.src = path
        })
    )

    try {
      ;[dilationSrc, voronoiSrc, mixedSrc] = await Promise.all(images)
    } catch (ev) {
      console.log(ev)
      alert('Failed to load ' + ev)

      close()

      return
    }

    loaded = true
  })

  function close() {
    morph.set(null)
    loaded = false
  }
</script>

<div class="Preview" in:fade={{ duration: 200 }} out:fade={{ duration: 200 }} on:click={close}>
  {#if loaded}
    <div class="morph dilation">
      <p>Dilation Morph</p>
      <img src={dilationSrc} alt="Dilation morph" width={480} height={480} />
    </div>
    <div class="morph voronoi">
      <p>Voronoi Morph</p>
      <img src={voronoiSrc} alt="Voronoi morph" width={480} height={480} />
    </div>
    <div class="morph mixed">
      <p>Mixed Morph</p>
      <img src={mixedSrc} alt="Mixed morph" width={480} height={480} />
    </div>
  {:else}
    Loading...
  {/if}
</div>

<div class="close" in:fade={{ duration: 200 }} out:fade={{ duration: 200 }} on:click={close}>
  Close &times;
</div>

<style>
  .Preview {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    display: flex;
    flex-direction: column;
    align-items: center;

    padding: 3rem 0 1rem;

    overflow: auto;

    gap: 1rem;

    background: rgba(255, 255, 255, 1);
  }

  .morph {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
  }

  .morph p {
    flex: 1;
    margin: 0;

    min-width: 150px;
    text-align: right;
  }

  img {
    width: 180px;
    height: 180px;
  }

  @media screen and (min-width: 450px) {
    img {
      width: 280px;
      height: 280px;
    }
  }

  @media screen and (min-width: 800px) {
    .Preview {
      padding: 0 1rem;

      flex-direction: row;
      justify-content: center;
    }

    .morph {
      flex: 1;
      flex-direction: column;
    }

    .morph p {
      text-align: center;
    }

    img {
      width: 100%;
      height: auto;
    }
  }

  .close {
    box-sizing: border-box;

    position: fixed;
    top: 0.5rem;
    right: 0.5rem;

    cursor: pointer;

    color: var(--accent);
    font-size: 1.5rem;
    text-align: right;
  }

  .close:hover {
    text-decoration: underline;
  }
</style>
