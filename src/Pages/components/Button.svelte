<script>
  import { gifs, numberOfGifs } from "../../store/store.js";
  import { createEventDispatcher } from "svelte";
  export let label = "NEXT";

  const dispatch = createEventDispatcher();
</script>

<style>
  .brk-btn {
    position: relative;
    background: none;
    color: rgb(165, 85, 255);
    text-transform: uppercase;
    text-decoration: none;
    border: 0.2em solid rgb(165, 85, 255);
    padding: 0.5em 1em;
  }
  .brk-btn::before {
    content: "";
    display: block;
    position: absolute;
    width: 10%;
    background: #222;
    height: 0.3em;
    right: 20%;
    top: -0.21em;
    transform: skewX(-45deg);
    -webkit-transition: all 0.45s cubic-bezier(0.86, 0, 0.07, 1);
    transition: all 0.45s cubic-bezier(0.86, 0, 0.07, 1);
  }
  .brk-btn::after {
    content: "";
    display: block;
    position: absolute;
    width: 10%;
    background: #222;
    height: 0.3em;
    left: 20%;
    bottom: -0.25em;
    transform: skewX(45deg);
    -webkit-transition: all 0.45 cubic-bezier(0.86, 0, 0.07, 1);
    transition: all 0.45s cubic-bezier(0.86, 0, 0.07, 1);
  }
  .brk-btn:hover::before {
    right: 80%;
  }
  .brk-btn:hover::after {
    left: 80%;
  }
</style>

<link
  href="https://fonts.googleapis.com/css?family=Dosis:700"
  rel="stylesheet" />

<button class="brk-btn" on:click={() => dispatch('active')}>{label}</button>
