<script lang="ts">
  import type { IndexMonster } from "./+page";
  import { goto } from "$app/navigation";
  import { page } from "$app/stores";

  export let monster: IndexMonster;
  export let isInteractive: boolean = false;

  const updateSearchParams = (key: string, value: string) => {
    const searchParams = new URLSearchParams($page.url.searchParams);
    searchParams.set(key, value);
    goto(`?${searchParams.toString()}`);
  };
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="monster">
  <div
    on:click={() =>
      isInteractive ? updateSearchParams("monster_id", monster.id) : () => {}}
  >
    <div class="monster-content">
      <img src={monster.image} alt={monster.name} />
      {monster.name}
    </div>
    <div class="monster-id">
      {monster.id}
    </div>
  </div>
  {#if isInteractive}
    <div on:click={() => updateSearchParams("monster_id2", monster.id)}>
      Add Monster 2
    </div>
  {/if}
</div>

<style>
  .monster-id {
    position: absolute;
    top: 8px;
    left: 8px;
    font-size: 0.8em;
    color: #aaa;
  }
  .monster {
    width: 100px;
    margin: 10px;
    padding: 10px;
    position: relative;
    cursor: pointer;
    border-radius: 10px;
    background: #eee;
    box-shadow: 20px 20px 60px #cacaca, -20px -20px 60px #ffffff;
  }
  .monster:hover {
    background: linear-gradient(145deg, #d6d6d6, #ffffff);
    box-shadow: 20px 20px 60px #cacaca, -20px -20px 60px #ffffff;
  }
  .monster-content {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
