<script lang="ts">
  import { onMount, createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  const close = () => dispatch('close');

  export let url;

  let spell;

  onMount(async () => {
    const res = await fetch(`https://www.dnd5eapi.co${url}`);

    spell = await res.json();
  });

  const handleKeyDown = (event) => {
    if (event.key === 'Escape') {
      close();
      return;
    }
  };
</script>

<svelte:window on:keydown={handleKeyDown} />

<div class="modal-bg" on:click={close} />

{#if spell}
  <div class="modal">
    <span class="spell-name">{spell.name}</span>
    <div class="spell-statblock">
      <div class="spell-stat">
        <p><strong>Level:</strong></p>
        <p>{spell.level}</p>
      </div>
      <div class="spell-stat">
        <p><strong>Casting Time:</strong></p>
        <p>{spell.casting_time}</p>
      </div>
      <div class="spell-stat">
        <p><strong>Range/Area:</strong></p>
        <p>{spell.range}</p>
      </div>
      <div class="spell-stat">
        <p><strong>Components:</strong></p>
        {#each spell.components as component}
          <span>{component}</span>
        {/each}
      </div>
      <div class="spell-stat">
        <p><strong>Duration:</strong></p>
        <p>{spell.duration}</p>
      </div>
      <div class="spell-stat">
        <p><strong>School:</strong></p>
        <p>{spell.school.name}</p>
      </div>
      <div class="spell-stat">
        <p><strong>Concentration:</strong></p>
        <p>{spell.concentration ? 'Yes' : 'No'}</p>
      </div>
      <div class="spell-stat">
        {#if spell.damage}
          <p><strong>Damage Type:</strong></p>
          <p>{spell.damage.damage_type.name}</p>
        {:else}
          <p><strong>Ritual:</strong></p>
          <p>{spell.ritual ? 'Yes' : 'No'}</p>
        {/if}
      </div>
    </div>
    <div class="seperator" />
    <div class="spell-desc">
      {#each spell.desc as desc}
        <p>{desc}</p>
      {/each}
    </div>
    <br />
    {#if spell.higher_level}
      <div class="spell-highlevel">
        <p><strong>Higher level</strong></p>
        <p>{spell.higher_level}</p>
      </div>
    {/if}
    {#if spell.material}
      <p class="spell-material">{spell.material}</p>
    {/if}
  </div>
{:else}
  <p>...</p>
{/if}

<style>
  p {
    margin: 0;
  }

  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.3);
  }

  .modal {
    position: absolute;
    left: 50%;
    top: 50%;
    width: calc(100vw - 4em);
    max-width: 32em;
    max-height: calc(100vh - 4em);
    overflow: auto;
    transform: translate(-50%, -50%);
    padding: 1em;
    border-radius: 0.2em;
    background: white;
  }

  .spell-name {
    text-align: center;
    font-size: 22px;
    margin-bottom: 4px;
  }

  .spell-statblock {
    display: flex;
    flex-wrap: wrap;
  }

  .spell-stat {
    flex: 25%;
    text-align: left;
  }

  .seperator {
    background-color: #ff3e00;
    height: 3px;
    margin: 12px 0;
    outline: none;
  }

  .spell-desc {
    text-align: left;
  }
  .spell-highlevel {
    text-align: left;
  }

  .spell-material {
    text-align: left;
    font-size: 14px;
    color: #ccc;
  }
</style>
