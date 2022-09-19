<script>
  import Array from "./Array.svelte";
  import Vector from "./Vector.svelte";

  let array_view = true;
</script>

<button
  on:click={() => {
    array_view = !array_view;
  }}>Swap</button
>
{#if array_view}
  <Array />
{:else}
  <Vector />
{/if}

<style></style>
