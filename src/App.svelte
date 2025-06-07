<script>
  import Card from './lib/Card.svelte';
  import { pokemons } from './lib/pokemonData';

  
  let pokemonData = $state(pokemons);
  let selectedPokemonIds = $state([]);
  let currScore = $state(0);
  let bestScore = $state(0);
  let modalDialogMessage = $state("");

  function shuffle(arr) {
    let shuffled = arr
      .map((val) => ({ val, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ val }) => val);
    return shuffled;
  }
  

</script>

<main>
  <dialog id="game-over-dialog">
        <div>
          <h1></h1>
          <button>Reset</button>
        </div>
      </dialog>
  <ul>
    {#each pokemonData as pokemon}
      <li>{pokemon.id}, {pokemon.name}</li>
      
      
    {/each}
    <button onclick={() => pokemonData=  shuffle(pokemonData)}>Shuffle the list</button>
  </ul>
  
</main>

<style>
  #game-over-dialog {
    position: fixed;
    left: 50%;
    top: 50%;
    -ms-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    border-radius: 1em;
    background-color: var(--bg-colour-1);
    box-shadow: var(--shadow-elevation-high);
  }
  
  #game-over-dialog > div {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
 
</style>
