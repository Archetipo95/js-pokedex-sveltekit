<script lang="ts">
  import type { PageData } from "./$types";
  import { page } from "$app/stores";
  import { generations } from "./generations";
  import Monster from "./Monster.svelte";
  import { goto } from "$app/navigation";

  export let data: PageData;

  let form = {
    searchString: "",
  };

  let searchString = "";

  $: selectedMonsters = data.monsters.filter((monster) => {
    return monster.name.toLowerCase().includes(searchString.toLowerCase());
  });

  $: monsterId = $page.url.searchParams.get("monster_id") || "";
  $: monster = data.monsters.find((monster) => monster.id === monsterId);
  $: monsterId2 = $page.url.searchParams.get("monster_id2") || "";
  $: monster2 = data.monsters.find((monster) => monster.id === monsterId2);

  $: selectedGenerationId = $page.url.searchParams.get("generation_id") || "";

  const submitSearch = (e: Event) => {
    searchString = form.searchString;
  };

  const updateSearchParams = (key: string, value: string) => {
    const searchParams = new URLSearchParams($page.url.searchParams);
    searchParams.set(key, value);
    goto(`?${searchParams.toString()}`);
  };
</script>

{#if monster}
  <Monster {monster} />
{/if}
{#if monster2}
  <Monster monster={monster2} />
{/if}

<div class="generations">
  <button
    class="generation"
    class:active={selectedGenerationId == "all"}
    on:click={() => updateSearchParams("generation_id", "all")}
  >
    All
  </button>

  {#each generations as generation (generation.id)}
    <button
      class="generation"
      class:active={selectedGenerationId === generation.id.toString()}
      on:click={() =>
        updateSearchParams("generation_id", generation.id.toString())}
    >
      {generation.main_region}
    </button>
  {/each}
</div>

<form class="search-form" on:submit|preventDefault={submitSearch}>
  <input
    class="search-field"
    type="text"
    bind:value={form.searchString}
    placeholder="Pokemon Name"
  />
  <input type="submit" value="Search" />
</form>

<div class="monsters">
  {#each selectedMonsters as monster (monster.id)}
    <Monster {monster} isInteractive />
  {/each}
</div>

<style>
  .generations {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }
  .generation {
    margin: 5px;
    padding: 5px 10px;
    border: 1px solid black;
    background-color: #f9f9f9;
    color: #333;
    cursor: pointer;
  }
  .generation:hover {
    background-color: #eee;
  }

  .generation.active {
    background-color: #333;
    color: #fff;
  }
  .monsters {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }
  .search-form {
    display: flex;
    justify-content: center;
    margin: 20px 0;
  }
  .search-form input[type="text"] {
    padding: 5px 10px;
    width: 200px;
    border-radius: 5px;
    border: 1px solid #333;
  }

  .search-form input[type="submit"] {
    padding: 5px 10px;
    border-radius: 5px;
    border: 1px solid #333;
    background-color: #333;
    color: #fff;
    margin-left: 10px;
    cursor: pointer;
  }
</style>
