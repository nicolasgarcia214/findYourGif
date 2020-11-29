<script>
  import Chart from "./components/Chart.svelte";
  import Search from "./components/Search.svelte";
  import { fade } from "svelte/transition";
  import { numberOfGifs, names } from "../store/store.js";
</script>

<div class="Main">
  {#if $numberOfGifs.length > 2}
    <div transition:fade>
      <Chart />
    </div>
  {/if}
  <Search />
</div>
