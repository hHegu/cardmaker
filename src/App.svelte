<script>
  import { onMount } from "svelte";
  import _ from "lodash";
  import Lander from "./pages/lander.svelte";
  import CardEditor from "./pages/card-editor.svelte";
  import CardList from "./pages/card-list.svelte";

  const LOCAL_STORAGE_NAME = "";

  let currentCard;
  let page = "card-editor";
  let cards = [];

  onMount(async () => {
    cards = loadCardsFromStorage();
  });

  const loadCardsFromStorage = () => {
    const savedCards = localStorage.getItem(LOCAL_STORAGE_NAME);
    try {
      const cardsJSON = JSON.parse(savedCards);
      return cardsJSON || [];
    } catch {
      return [];
    }
  };

  const saveCardsToStorage = () => {
    localStorage.setItem(LOCAL_STORAGE_NAME, JSON.stringify(cards));
  };

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
    saveCardsToStorage();
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
      abilities: [],
      actionCards: {
        attacks: 0,
        defends: 0,
        specials: 0
      }
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
    saveCardsToStorage();
  };

  const copyCard = ({ detail }) => {
    const existingIndex = _.findIndex(cards, card => card.id === detail.id);
    const newCard = _.cloneDeep(detail);
    newCard.id = _.uniqueId();
    cards.splice(existingIndex, 0, newCard);
    cards = cards;
    saveCardsToStorage();
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
