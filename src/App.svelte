<script>
  import _ from "lodash";
  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  import CardList from "./pages/card-list.svelte";

  let currentCard;
  let page = "card-editor";
  const cards = [];

  const saveCard = ({ detail }) => {
    const { id } = detail;
    const existingCardId = _.findIndex(cards, card => id === card.id);
    if (existingCardId >= 0) {
      cards[existingCardId] = detail;
    } else {
      cards.push(_.cloneDeep(detail));
    }
    cards = cards;
    closeCardEditor();
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
  };

  const closeCardEditor = () => {
    currentCard = undefined;
  };

  const editCard = ({ detail }) => {
    currentCard = _.cloneDeep(detail);
  };

  const deleteCard = ({ detail }) => {
    cards.splice(_.findIndex(cards, card => card.id === detail.id), 1);
    cards = cards;
  };

  const copyCard = ({ detail }) => {
    const existingIndex = _.findIndex(cards, card => card.id === detail.id);
    const newCard = _.cloneDeep(detail);
    newCard.id = _.uniqueId();
    cards.splice(existingIndex, 0, newCard);
    cards = cards;
  };
</script>

<style>
  main {
    font-family: Roboto;
    height: 100%;
  }
  .cardmaker {
    display: flex;
    height: 100%;
  }
</style>

<main>
  {#if page === 'card-editor'}
    <div class="cardmaker">
      <CardEditor
        card={currentCard}
        on:cancel={closeCardEditor}
        on:new={newCard}
        on:save={card => {
          saveCard(card);
          page = 'card-editor';
        }} />
      <CardList
        {cards}
        on:back={() => (page = 'lander')}
        on:edit={editCard}
        on:copy={copyCard}
        on:delete={deleteCard} />
    </div>
  {:else}
    <Lander
      on:card-editor={newCard}
      on:card-list={() => (page = 'card-editor')} />
  {/if}
</main>
