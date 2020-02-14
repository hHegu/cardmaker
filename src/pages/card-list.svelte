<script>
  import _ from "lodash";
  import uuidv1 from "uuid/v1";

  import Card from "../components/card.svelte";
  import CardTypeTag from "../components/card-type-tag.svelte";

  import Icon from "svelte-awesome";

  import { faChevronDown } from "@fortawesome/free-solid-svg-icons/faChevronDown";
  import { faChevronUp } from "@fortawesome/free-solid-svg-icons/faChevronUp";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let newTestingCard = () => ({
    id: _.uuidv1(),
    cost: null,
    name: null,
    type: "creature",
    backgroundImage: "https://i.redd.it/px75th6i2nn11.jpg",
    xp: 0,
    gold: 0,
    abilities: [
      {
        type: "attack",
        abilityNumber: 1,
        description:
          "Deal <b>5 damage</b> to hero with the lowest health. That player discards a card."
      },
      {
        type: "attack",
        abilityNumber: 2,
        description: "Deal <b>3 damage</b> to all heroes."
      },
      {
        type: "defend",
        abilityNumber: 1,
        description: "Give 4 block to all allies."
      },
      {
        type: "special",
        abilityNumber: 1,
        description:
          "Destroy an ally with the lowest health. This creature heals the amount of that creatures max hp."
      }
    ],
    actionCards: {
      attacks: 0,
      defends: 0,
      specials: 0
    }
  });

  export let cards = [];

  const categorize = cards =>
    _.reduce(
      cards,
      (result, card) => {
        const category = _.find(
          result,
          category => category.categoryName === card.type
        );
        if (category) {
          category.cards.push(card);
        } else {
          result.push({ categoryName: card.type, cards: [card] });
        }
        return result;
      },
      []
    );

  $: categorizedCards = categorize(cards);

  $: sortedCards = _.sortBy(cards, 'type')
</script>

<style>
  .card-list {
    display: flex;
    flex: 1;
    flex-direction: row;
    overflow: auto;
    flex-wrap: wrap;
    max-height: 100%;
    background: var(--background-color);
  }

  .card-container {
    padding: 0.5rem;
  }

  .card {
    position: relative;
  }

  .testing {
    position: fixed;
    top: 1rem;
    right: 1rem;
  }

  .card-overlay {
    width: 100%;
    height: 100%;
    background: rgb(255, 255, 255, 0.75);
    position: absolute;
    top: 0;
    left: 0;
    transition: 0.2s;
    opacity: 0;
    justify-content: center;
    align-items: center;
    display: flex;
    visibility: hidden;
  }

  .card:hover .card-overlay {
    visibility: visible;
    opacity: 1;
  }

  @media print {
    .card-container {
      padding: 0;
    }
    .testing {
      display: none;
    }
    .card-list {
      max-height: none;
      background: white;
      overflow: visible;
    }
    .card-container:nth-child(9n) {
      padding-bottom: calc(var(--a4-height) - var(--card-height) * 3);
    }
  }
</style>

<div class="card-list">
  {#each sortedCards as card}
    <div class="card-container">
      <CardTypeTag type={card.type} />
      <div class="card">
        <Card {card} />
        <div class="card-overlay">
          <button on:click={() => dispatch('edit', card)}>Edit</button>
          <button
            on:click={() => dispatch('copy', card)}
            style="margin: 0 0.5rem">
            Copy
          </button>
          <button on:click={() => dispatch('delete', card)}>Delete</button>
        </div>
      </div>
    </div>
  {/each}
</div>
<div class="testing">
  <button
    on:click={() => {
      cards.push(newTestingCard());
      cards = cards;
    }}>
    Testing card
  </button>
</div>
