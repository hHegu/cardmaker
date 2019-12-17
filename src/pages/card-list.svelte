<script>
  import _ from "lodash";
  import Card from "../components/card.svelte";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let newTestingCard = () => ({
    id: _.uniqueId(),
    cost: undefined,
    name: undefined,
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
    ]
  });

  export let cards = [];

  const categorize = cards =>
    _.reduce(
      cards,
      (result, card) => {
        if (!result[card.type]) result[card.type] = [];
        result[card.type].push(card);
        return result;
      },
      new Object()
    );

  $: categorizedCards = categorize(cards);
</script>

<style>
  .card-list {
    display: flex;
    flex-wrap: wrap;
  }
  .card-container {
    padding: 1rem;
  }

  .card {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .testing {
    position: fixed;
    bottom: 1rem;
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
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .card:hover .card-overlay {
    opacity: 1;
  }

  @media print {
    .card-container {
      padding: 0;
    }
  }
</style>

<div class="card-list">
  {#each cards as card}
    <div class="card-container">
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
  <button on:click={() => dispatch('new')}>New card</button>
  <button on:click={() => dispatch('back')}>Landing page</button>
</div>
