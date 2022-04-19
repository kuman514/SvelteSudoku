<script>
  export let locked;
  export let number;
  export let fault;
  export let row;
  export let col;
</script>

<button
  class={`${locked ? 'locked' : ''} ${fault ? 'fault' : ''} ${(Math.floor(row / 3) + Math.floor(col / 3)) % 2 === 0 ? 'ordinary' : 'alternative'}` }
>
  {(1 <= number && number <= 9) ? number : ' '}
</button>

<style>
  button {
    all: unset;
    font-size: 4vh;
  }

  .ordinary {
    background-color: white;
  }

  .alternative {
    background-color: #fcebc0;
  }

  .locked {
    font-weight: bold;
  }

  .fault {
    color: red;
  }
</style>
