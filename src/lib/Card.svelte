<script>
    let {name, id, imageUrl, handleClick} = $props();
</script>

<div class="pokemon-card" onclick={handleClick}>
    <span>{name}</span>
    <img src={imageUrl} alt={name}>
</div>

<style>
    .pokemon-card {
      width: 10em;
      height: 10em;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background-color: var(--bg-colour-2);
      border-radius: 1em;
      border-width: 0.25em;
      border-style: solid;
      transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
    }
    
    .pokemon-card:hover {
      transform: scale(1.05);
      border-color: var(--border-colour);
      box-shadow: var(--shadow-elevation-medium);
    }
    
    .pokemon-card img {
      height: 8em;
      width: 8em;
      transition: transform 0.2s ease;
    }
    
    .pokemon-card:hover img {
      transform: scale(1.15);
    }

</style>