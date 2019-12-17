<script>
  import _ from "lodash";
  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  let page = "card-editor";
  const cards = [];
  const saveCard = ({ detail }) => {
    cards.push(_.clone(detail));
    console.log(cards);
  };
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
  {:else if page === 'card-editor'}
    <CardEditor
      on:cancel={() => (page = 'lander')}
      on:save={card => {
        saveCard(card);
        page = 'lander';
      }} />
  {:else}
    <Lander
      on:card-editor={() => (page = 'card-editor')}
      on:card-list={() => (page = 'card-list')} />
  {/if}
</main>
