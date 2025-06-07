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

  async function getPokemonData() {
    try {
      const fetchedResults = await Promise.all(
        pokemons.map(async (pokemon) => {
          console.log(pokemon);
          const response = await fetch(
            `https://pokeapi.co/api/v2/pokemon/${pokemon.name}`
          );
          const data = await response.json();
          return {
            ...pokemon,
            imageUrl: data.sprites.front_default,
            name: pokemon.name[0].toUpperCase() + pokemon.name.slice(1),
          };
        })
      );

      pokemonData = shuffle(fetchedResults);
    } catch (err) {
      console.log("Error fetching from Pokemon API: ", err);
    }
  }

  $effect(() => {
    getPokemonData();
  })

  function handleGame(pokemonId){
    if (selectedPokemonIds.includes(pokemonId) == true){
      modalDialogMessage = "Game Over!";
      document.querySelector("#game-over-dialog").showModal();
    } else {
      selectedPokemonIds.push(pokemonId);
      console.log(selectedPokemonIds);
      currScore += 1;
      pokemonData = shuffle(pokemonData);
    }
    if (currScore == 8){
      modalDialogMessage = "You won, congrats!";
      document.querySelector("#game-over-dialog").showModal();
    }
  }

  function resetGame(){
    if (currScore > bestScore){
      bestScore = currScore;
    }
    currScore = 0;
    selectedPokemonIds = [];
    modalDialogMessage = "";
    pokemonData = shuffle(pokemonData);
    document.querySelector("#game-over-dialog").close()
  }
  

</script>

<main>
  <dialog id="game-over-dialog">
    <div>
      <h1>{modalDialogMessage}</h1>
      <button onclick={resetGame}>Reset</button>
    </div>
  </dialog>
  <h1>Pokemon Memory Game!</h1>
  <h2>
    Get points by cliking on an image, but make sure you only click an image
    once!
  </h2>
  <div id="score-container">
    <h3>
      Current Score: {currScore} | Best Score: {bestScore}
    </h3>
  </div>
  <div id="pokemon-card-grid">
    {#each pokemonData as pokemon (pokemon.id)}
      <Card
        {...pokemon}
        handleClick={() => handleGame(pokemon.id)}
      ></Card>
      
      
    {/each}
    <button onclick={() => pokemonData=  shuffle(pokemonData)}>Shuffle the deck</button>
  </div>
  
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

  #pokemon-card-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    row-gap: 1em;
    column-gap: 1em;
    padding: 1em;
  }

  button {
  background-color: var(--bg-colour-2);
  border: solid;
  border-radius: 0.5em;
}
  
 
</style>
