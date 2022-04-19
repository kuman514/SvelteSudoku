<script>
  export let selectedRow;
  export let selectedCol;
  export let locked;
</script>

<div>
  <div class="status">
    {#if (selectedRow < 0 || selectedRow >= 9 || selectedCol < 0 || selectedCol >= 9)}
      Not Selected
    {:else if locked}
      A Fixed Number
    {:else}
      [{selectedCol + 1}, {selectedRow + 1}] Selected
    {/if}
  </div>
  <div>
    {#each [1, 2, 3] as num}
      <button id={`digitButton-${num}`}>
        {num}
      </button>
    {/each}
  </div>
  <div>
    {#each [4, 5, 6] as num}
      <button id={`digitButton-${num}`}>
        {num}
      </button>
    {/each}
  </div>
  <div>
    {#each [7, 8, 9] as num}
      <button id={`digitButton-${num}`}>
        {num}
      </button>
    {/each}
  </div>
  <div>
    <button id="digitButton-0">
      Erase
    </button>
  </div>
</div>

<style>
  button {
    all: unset;
    width: 4.5vh;
    height: 4.5vh;
    max-width: 9vw;
    max-height: 9vw;
    font-size: 2.5vh;
    font-variant-numeric: tabular-nums;
    background-color: #fcebc0;
  }

  .status {
    font-size: 2.5vh;
  }

  #digitButton-0 {
    width: 13.5vh;
    max-width: 27vw;
  }
</style>
