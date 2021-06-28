<script lang="ts">
  import SpellModal from './SpellModal.svelte';

  export let name;
  export let url;

  let showModal = false;
</script>

<li on:click={() => (showModal = true)}>
  {name}
</li>

{#if showModal}
  <SpellModal {url} on:close={() => (showModal = false)} />
{/if}

<style>
  li {
    text-align: left;
    cursor: pointer;
    list-style: none;
    padding: 1px 0 1px 3px;
    border-bottom: 1px solid #ccc;
    background-color: white;
  }

  li:hover {
    background-color: #ff3e00;
    color: #fff;
  }
</style>
