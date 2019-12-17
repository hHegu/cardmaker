<script>
  import _ from "lodash";
  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  import CardList from "./pages/card-list.svelte";

  let currentCard = {};
  let page = "card-list";
  const cards = [];

  const saveCard = ({ detail }) => {
    const { id } = detail;
    const existingCardId = _.find(cards, card => id === card.id);
    if (existingCardId) {
      cards[existingCardId] = detail;
    } else {
      cards.push(_.clone(detail));
    }
    cards = cards;
  };

  const newCard = () => {
    currentCard = {
      id: _.uniqueId(),
      cost: undefined,
      name: undefined,
      type: "creature",
      backgroundImage: undefined,
      xp: 0,
      gold: 0,
      abilities: []
    };
    page = "card-editor";
  };

  const editCard = ({ detail }) => {
    currentCard = detail;
    page = "card-editor";
  };

  const deleteCard = ({ id }) => {
    cards.splice(_.findIndex("id", id), 1);
    cards = cards;
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
    <CardList
      {cards}
      on:back={() => (page = 'lander')}
      on:new={newCard}
      on:edit={editCard} />
  {:else if page === 'card-editor'}
    <CardEditor
      card={currentCard}
      on:cancel={() => (page = 'lander')}
      on:save={card => {
        saveCard(card);
        page = 'card-list';
      }} />
  {:else}
    <Lander
      on:card-editor={newCard}
      on:card-list={() => (page = 'card-list')} />
  {/if}
</main>
