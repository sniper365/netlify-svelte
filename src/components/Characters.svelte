<script>
  import { onMount } from 'svelte';

  let query = '';
  let characters = [];
  let endpoint = 'https://rickandmortyapi.com/api/character';

  onMount(async () => {
    const res = await fetch(endpoint);
    const data = await res.json();
    characters = data.results;
  });

  async function search() {
    let filter = `https://rickandmortyapi.com/api/character/?name=${query}`;
    const res = await fetch(filter);
    const data = await res.json();
    characters = data.results;
  }
</script>

<div class="contentSearch">
  <input
    type="search"
    bind:value={query}
    on:keydown={search}
    placeholder="Search by Name"
  />
</div>

<main>
  {#each characters as character}
    <figure>
      <picture>
        <img src={character.image} alt="Imagen de {character.name}" loading="lazy" decoding="async">
      </picture>
      <figcaption>
        <section>
          <h1 class="name">{character.name}</h1>
          <h2>{character.species}</h2>
          <h3>{character.status} </h3>
          <span>{character.location.name}</span>
        </section>
      </figcaption>
    </figure>
  {:else}
    <h1 class="title">loading...</h1>
  {/each}
</main>

<style>
  .contentSearch {
    display: flex;
    justify-content: center;
    padding: 0 0 3em 0;
  }

  input[type="search"] {
    width: 230px;
    padding: 0.7em;
    font-size: inherit;
	  border: 1px solid #b8b8b8;
	  border-radius: 5px
  }

  main {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  figure {
    display: flex;
    width: 100%;
    max-width: 300px;
    flex-direction: column;
    background: hsl(0 0% 70%);
    border-radius: 7px;
    overflow: hidden;
    margin: 10px;
  }

  section {
    color: black;
    padding: 0.1em 1em 1em 1em;
    inset: 0;
    margin-top: -90px;
    backdrop-filter: blur(40px);
    -webkit-backdrop-filter: blur(40px);
    -moz-backdrop-filter: blur(40px);
  }

  .name {
    text-transform: uppercase;
    font-style: italic;
  }

  img {
    max-width: 100%;
  }
</style>