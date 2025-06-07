<script>
  import Card from './lib/Card.svelte';
  import { pokemons } from './lib/pokemonData';
  
</script>

<main>
  <ul>
    {#each pokemons as pokemon}
      <li>{pokemon.id}, {pokemon.name}</li>
      
      
    {/each}
  </ul>
  
</main>

<style>
 
</style>
