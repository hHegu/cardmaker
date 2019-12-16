<script>
  import Lander from "./pages/lander.svelte";
  import NewCard from "./pages/new-card.svelte";
  let page = "new-card";
</script>

<style>
  main {
    font-family: Roboto;
    height: 100%;
  }
</style>

<main>
  {#if page === 'card-list'}
    <!-- <CardList /> -->
    <div>Card list</div>
  {:else if page === 'new-card'}
    <NewCard on:back={() => (page = 'lander')} />
  {:else}
    <Lander
      on:new-card={() => (page = 'new-card')}
      on:card-list={() => (page = 'card-list')} />
  {/if}
</main>
