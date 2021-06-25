<script lang="ts">
  let searchTerm = '';
</script>

<div class="search-container">
  <input placeholder="Search for a spell" bind:value={searchTerm} />
</div>

<style>
  .search-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  input {
    margin: 0px;
    width: 25%;
  }
</style>
