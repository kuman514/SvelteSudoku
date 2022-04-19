<script>
  import Tile from './Tile.svelte';
  export let boardLine;
  export let lockedLine;
  export let faultLine;
  export let row;
  export let selectedRow;
  export let selectedCol;
</script>

<div>
  {#each boardLine as tile, col}
    <Tile
      locked={lockedLine[col]}
      number={tile}
      fault={lockedLine[col] ? false : faultLine[col]}
      {row}
      {col}
      {selectedRow}
      {selectedCol}
    />
  {/each}
</div>
