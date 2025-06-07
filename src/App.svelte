<script>
  import Card from './lib/Card.svelte';
  import { pokemons } from './lib/pokemonData';

  function shuffle(arr) {
    let shuffled = arr
      .map((val) => ({ val, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ val }) => val);
    return shuffled;
  }
  let pokemonData = $state(pokemons);

</script>

<main>
  <ul>
    {#each pokemonData as pokemon}
      <li>{pokemon.id}, {pokemon.name}</li>
      
      
    {/each}
    <button onclick={() => pokemonData=  shuffle(pokemonData)}>Shuffle the list</button>
  </ul>
  
</main>

<style>
 
</style>
