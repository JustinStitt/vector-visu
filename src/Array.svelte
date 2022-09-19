<script>
  import { slide, fade } from "svelte/transition";
  let array_size = 5,
    to_submit = 0,
    lfi = 0;
  // $: arr = Array(array_size).fill(0);
  let arr = Array(array_size).fill(0);
  $: {
    arr = [
      ...arr.slice(0, Math.min(arr.length, array_size)),
      ...Array(Math.max(array_size - arr.length, 0)).fill(0),
    ];
  }

  $: code_preamble = `<span id="type">int</span> arr[<span id="idx">${arr.length}</span>];`;
  let lines_of_code = Array(arr.length).fill("");

  $: {
    for (let i = 0; i < lines_of_code.length; ++i) {
      if (i >= arr.length) lines_of_code[i] = "";
    }
  }

  let show_indices = false;

  const convertChangeToCode = (idx) => {
    let code = `arr[<span id="idx">${idx}</span>] = <span id="val">${
      isNaN(arr[idx]) ||
      arr[idx].length == 0 ||
      (arr[idx].length > 1 && arr[idx][0] === "0")
        ? "❌"
        : arr[idx]
    }</span>;`;
    lines_of_code[idx] = code;
  };
</script>

<div class="container">
  <h1>Array Visu</h1>
  <label for="array-size">Size: {array_size}</label>
  <input
    id="array-size"
    type="range"
    min="1"
    max="10"
    bind:value={array_size}
  />
  <div class="box-container">
    {#each arr as box, i}
      {#if show_indices}
        <span transition:fade class="index">{i}</span>
      {/if}
      <div
        class="box"
        transition:slide
        contenteditable="true"
        spellcheck="false"
        bind:textContent={arr[i]}
        on:input={() => {
          convertChangeToCode(i);
        }}
      >
        {box}
      </div>
    {/each}
  </div>
  <div class="code-and-options">
    <ol class="code" transition:slide>
      <li>
        <h3>
          {@html code_preamble}
        </h3>
      </li>
      {#each lines_of_code as line, i}
        {#if line != undefined && line.length}
          <li transition:fade>
            <h3>
              {@html line}
            </h3>
          </li>
        {/if}
      {/each}
    </ol>
    <label id="label-cb">
      Show Indices
      <input type="checkbox" id="show-indices-cb" bind:checked={show_indices} />
    </label>
  </div>
</div>

<style>
  .box {
    min-width: 7vw;
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
  }
  .box-container {
    display: flex;
    padding: 10px;
    margin-right: auto;
    margin-left: 17vw;
  }
  .container {
    width: 100%;
    height: 100%;
    overflow: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #array-size {
    width: 20%;
    border: 3px dotted palevioletred;
    color: #333;
    font-weight: 700;
    margin: 0;
    -moz-appearance: textfield;
    text-align: center;
  }

  .code {
    width: 10vw;
    background-color: #121212;
    font-family: Consolas;
    line-height: 0px;
    border-radius: 1vw;
    padding: 3vw 4vw 5vw 2vw;
    text-align: left;
    white-space: nowrap;
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  :global(h3 > #idx) {
    color: lightblue;
  }

  :global(h3 > #val) {
    color: palevioletred;
  }

  :global(h3 > #type) {
    color: goldenrod;
  }

  .index {
    position: relative;
    width: 0;
    left: 1%;
    top: 2%;
    color: rgba(211, 211, 211, 0.47);
    background-color: rebeccapurple;
  }

  .code-and-options {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  #label-cb {
    margin-left: 1vw;
  }

  li {
    margin-bottom: 2.5vh;
  }

  li::marker {
    font-family: Consolas;
    color: rgb(141, 136, 136);
  }
</style>
