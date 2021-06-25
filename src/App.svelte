<script lang="ts">
  import Header from './components/Header.svelte';
  import SpellSearch from './components/SpellSearch.svelte';
</script>

<main>
  <Header title={'Svelte Spells'} />
  <SpellSearch />
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
