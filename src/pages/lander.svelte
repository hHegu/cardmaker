<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
</script>

<style>
  lander {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }

  .buttonContainer {
    display: flex;
    max-width: 20rem;
    width: 20rem;
    justify-content: space-around;
    margin: 0 -0.5rem;
    padding-bottom: 10rem;
  }

  button {
    flex: 1;
    margin: 0 0.5rem;
  }
</style>

<lander>
  <h1>Card Maker</h1>
  <div class="buttonContainer">
    <button on:click={() => dispatch('card-editor')}>New Card</button>
    <button on:click={() => dispatch('card-list')}>List all</button>
  </div>
</lander>
