<script>
  import { slide, fade } from "svelte/transition";
  let size = 0;
  let capacity = 1;
  let vec = Array(capacity).fill("");

  let to_push = 0;
  let show_indices = false;

  const pushBack = () => {
    if (isNaN(to_push)) return;
    if (size >= capacity) {
      doubleCapacity();
    }
    vec[size] = to_push;
    size += 1;
  };

  const popBack = () => {
    //
    if (size < 1) return;
    vec[size - 1] = "";
    size -= 1;
    if (size < capacity / 2) {
      halfCapacity();
    }
  };

  const halfCapacity = () => {
    if (capacity < 2) return;
    for (let i = 0; i < capacity / 2; ++i) vec.pop();
    capacity = capacity / 2;
  };

  const doubleCapacity = () => {
    for (let i = 0; i < capacity; ++i) vec.push("");
    capacity *= 2;
  };
</script>

<div class="container">
  <h1>Vector Visu</h1>
  <div class="vec-container" transition:fade>
    {#each vec as elem, idx}
      {#if show_indices}
        <span transition:fade class="index">{idx}</span>
      {/if}
      <div class="box" contenteditable="true" transition:slide>
        {elem}
      </div>
    {/each}
  </div>
  <div class="below">
    <div class="stat">Size: {size}</div>
    <div class="stat">Capacity: {capacity}</div>
    <div class="stat">Load Factor: {size / capacity}</div>
    <div class="to-push" contenteditable="true" bind:textContent={to_push}>
      0
    </div>
    <button on:click={pushBack}>Push Back</button>
    <button on:click={popBack}>Pop Back</button>
    <label>
      Show Indices: <input type="checkbox" bind:checked={show_indices} />
    </label>
  </div>
</div>

<style>
  .vec-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 100%;
    padding: 10px;
    transition: 1.4s ease-in-out;
  }

  .box {
    min-width: 0.1%;
    width: 10%;
    max-width: 15%;
    aspect-ratio: 1;
    background-color: #121212;
    border: 2px solid aliceblue;
    color: palevioletred;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 300%;
    border-radius: 5%;
    transition: 1.4s ease-in-out;
  }

  .container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  .below {
    margin-top: 5vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .to-push {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 10vw;
    font-size: 2vw;
    background-color: rebeccapurple;
    border: 1px solid aliceblue;
    margin-bottom: 0.5vh;
  }

  .stat {
    align-self: start;
    margin-bottom: 1vh;
  }

  .index {
    position: relative;
    width: 0;
    left: 1%;
    top: 1vh;
    color: rgba(211, 211, 211, 0.47);
  }
</style>
