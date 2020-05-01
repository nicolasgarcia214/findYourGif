<script>
  import { fly } from "svelte/transition";
  import Button from "./Button.svelte";
  import Loader from "./Loader.svelte";
  import { gifs, numberOfGifs, names } from "../../store/store.js";

  let visible = false;
  setTimeout(() => {
    visible = true;
  }, 1000);

  let count = 0;
  let ref = 0;
  let search = "";
  let previousSearch = [];
  let totalGifs = {};
  const API_URL =
    "https://api.giphy.com/v1/gifs/search?limit=12&api_key=KmZWXurv5dQK6qA8Ma9aUM7nkrVf7jBi&rating=pg&q=";

  async function fetchGifsSearch() {
    event.preventDefault();
    $gifs = [];
    if (Object.keys(totalGifs).includes(search)) {
      $gifs = totalGifs[search].slice(0, 12);
    } else {
      const response = await fetch(`${API_URL}${search}`);
      const json = await response.json();
      $gifs = json.data.map(gif => gif.images.fixed_height.url);
      totalGifs[search] = $gifs;
      if (!$names.includes(search)) {
        $names = [...$names, search];
        $numberOfGifs = [...$numberOfGifs, json.pagination.total_count];
      }
    }
    count = 12;
    ref = 12;
  }

  async function fetchGifsNextButton() {
    event.preventDefault();
    if (count < ref) {
      $gifs = totalGifs[search].slice(count, count + 12);
      count += 12;
    } else {
      const response = await fetch(`${API_URL}${search}&offset=${count}`);
      const json = await response.json();
      $gifs = json.data.map(gif => gif.images.fixed_height.url);
      let storedValues = totalGifs[search];
      totalGifs[search] = storedValues.concat($gifs);
      count += 12;
      ref += 12;
    }
  }

  function getGifsBackButton() {
    event.preventDefault();
    if (count === 0) {
      return false;
    } else {
      $gifs = totalGifs[search].slice(count - 24, count - 12);
      count -= 12;
    }
  }
</script>

<style>
  .tb {
    display: table;
    width: 100%;
  }

  .td {
    display: table-cell;
    vertical-align: middle;
  }

  input,
  button {
    color: #fff;
    font-family: sans-serif;
    padding: 0;
    margin: 0;
    border: 0;
    background-color: transparent;
  }

  #cover {
    padding: 35px;
    background-color: #343062;
    border-radius: 20px;
    box-shadow: 0 10px 40px #2b275f, 0 0 0 20px #ffffffeb;
    transform: scale(0.6);
  }

  form {
    height: 96px;
  }

  input[type="text"] {
    width: 100%;
    height: 96px;
    font-size: 35px;
    line-height: 1;
  }

  input[type="text"]::placeholder {
    color: #cccccc;
  }

  #s-cover {
    width: 1px;
    padding-left: 35px;
  }

  button {
    position: relative;
    display: block;
    width: 84px;
    height: 96px;
    cursor: pointer;
  }

  #s-circle {
    position: relative;
    top: -8px;
    left: 0;
    width: 33px;
    height: 33px;
    margin-top: 0;
    border-width: 15px;
    border: 15px solid #fff;
    background-color: transparent;
    border-radius: 50%;
    transition: 0.5s ease all;
  }

  button span {
    position: absolute;
    top: 68px;
    left: 43px;
    display: block;
    width: 45px;
    height: 15px;
    background-color: transparent;
    border-radius: 10px;
    transform: rotateZ(52deg);
    transition: 0.5s ease all;
  }

  button span:before,
  button span:after {
    content: "";
    position: absolute;
    bottom: 0;
    right: 0;
    width: 45px;
    height: 15px;
    background-color: #fff;
    border-radius: 10px;
    transform: rotateZ(0);
    transition: 0.5s ease all;
  }

  #s-cover:hover #s-circle {
    top: -1px;
    width: 67px;
    height: 15px;
    border-width: 0;
    background-color: #fff;
    border-radius: 20px;
  }

  #s-cover:hover span {
    top: 50%;
    left: 56px;
    width: 25px;
    margin-top: -9px;
    transform: rotateZ(0);
  }

  #s-cover:hover button span:before {
    bottom: 11px;
    transform: rotateZ(52deg);
  }

  #s-cover:hover button span:after {
    bottom: -11px;
    transform: rotateZ(-52deg);
  }
  #s-cover:hover button span:before,
  #s-cover:hover button span:after {
    right: -6px;
    width: 40px;
    background-color: #fff;
  }
  .Buttons {
    display: grid;
    margin: 30px;
    gap: 20px;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    grid-template-columns: repeat(2, 1fr);
  }
  .Button {
    display: grid;
    gap: 20px;
    margin: 30px;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .grid-results {
    justify-content: center;
    display: grid;
    margin: auto;
    padding-bottom: 30px;
    gap: 30px;
    grid-template-columns: repeat(auto-fit, 300px);
  }
  .grid-results img {
    width: 100%;
    height: 300px;
    border-radius: 20px;
  }
  @media (min-width: 576px) {
    .grid-results {
      max-width: 540px;
    }
    input[type="text"] {
      font-size: 60px;
    }
    #s-circle {
      width: 43px;
      height: 43px;
    }
  }
  @media (min-width: 768px) {
    .grid-results {
      max-width: 720px;
    }
  }
  @media (min-width: 992px) {
    .grid-results {
      max-width: 2000px;
    }
  }
  @media (min-width: 1635px) {
    .grid-results {
      grid-template-columns: repeat(4, 350px);
    }
    .grid-results img {
      height: 350px;
    }
  }
</style>

{#if visible}
  <div transition:fly={{ y: 200, duration: 2000 }} id="cover">
    <form on:submit={fetchGifsSearch} method="get" action="">
      <div class="tb">
        <div class="td">
          <input
            bind:value={search}
            type="text"
            placeholder="Find your gif"
            required />
        </div>
        <div class="td" id="s-cover">
          <button type="submit">
            <div id="s-circle" />
            <span />
          </button>
        </div>
      </div>
    </form>
  </div>
{/if}

<div class="grid-results">
  {#each $gifs as gif}
    <img alt="gif" src={gif} />
  {/each}
</div>

{#if count >= 12}
  <div class={count >= 24 ? 'Buttons' : 'Button'}>
    {#if count >= 24}
      <Button on:active={getGifsBackButton} label="BACK" />
    {/if}
    <Button on:active={fetchGifsNextButton} label="NEXT" />
  </div>
{/if}
