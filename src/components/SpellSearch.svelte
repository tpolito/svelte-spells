<script lang="ts">
  import SpellItem from './SpellItem.svelte';
  let searchTerm = '';
  let searchTimer;
  let results = [];

  function handleKeyUp() {
    clearTimeout(searchTimer);

    searchTimer = setTimeout(() => {
      searchSpell(searchTerm);
    }, 500);
  }

  function handleKeyDown() {
    clearTimeout(searchTimer);
  }

  async function searchSpell(query: string) {
    const cleanQuery = query.trim();

    if (cleanQuery.length === 0 || cleanQuery === '') {
      results = [];
    } else {
      console.log('Searching for ' + cleanQuery);
      const res = await fetch(
        `https://www.dnd5eapi.co/api/spells/?name=${cleanQuery}`
      );

      let data = await res.json();
      results = data.results.slice(0, 5);
    }
  }
</script>

<div class="search-container">
  <div class="inner-container">
    <input
      placeholder="Search for a spell"
      bind:value={searchTerm}
      on:keydown={handleKeyDown}
      on:keyup={handleKeyUp}
    />
    {#if results.length === 0}
      <div />
    {:else}
      <ul class="search-results">
        {#each results as spell}
          <SpellItem {...spell} />
        {/each}
      </ul>
    {/if}
  </div>
</div>

<style>
  input {
    margin: 0;
    margin-top: 10px;
    width: 35%;
  }

  input:focus {
    outline-color: #ff3e00;
  }

  .inner-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    width: 65%;
    margin: 0 auto;
    border-radius: 5px;
    background-color: rgb(119, 119, 119, 0.28);
    height: 300px;
  }

  .search-results {
    margin: 0;
    padding: 0;
    width: 35%;
    border-left: 1px solid #ccc;
    border-right: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
    border-radius: 2px;
  }
</style>
