<script>
  export let selectedRow;
  export let selectedCol;
  export let locked;
</script>

<div>
  <div>
    {#if (selectedRow < 0 || selectedRow >= 9 || selectedCol < 0 || selectedCol >= 9)}
      Not Selected
    {:else if locked}
      A Fixed Number
    {:else}
      [{selectedCol + 1}, {selectedRow + 1}] Selected
    {/if}
  </div>
  <div>
    {#each [1, 2, 3, 4, 5, 6, 7, 8, 9] as num}
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
