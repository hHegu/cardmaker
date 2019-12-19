<script>
  import _ from "lodash";

  import { createEventDispatcher } from "svelte";

  import Icon from "fa-svelte";

  import { faTimes } from "@fortawesome/free-solid-svg-icons/faTimes";
  import { faArrowUp } from "@fortawesome/free-solid-svg-icons/faArrowUp";
  import { faArrowDown } from "@fortawesome/free-solid-svg-icons/faArrowDown";
  import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";
  import { faMinus } from "@fortawesome/free-solid-svg-icons/faMinus";

  import Card from "../components/card.svelte";
  import AbilityList from "../components/ability-list.svelte";

  const dispatch = createEventDispatcher();

  export let card = {
    cost: undefined,
    name: undefined,
    type: "creature",
    backgroundImage: undefined,
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
  };

  let scale = 1;
  $: scalePercentage = `${_.round(scale * 100)}%`;

  const cardTypes = [
    { id: "creature", text: "Creature" },
    { id: "item", text: "Item" }
  ];

  const zoom = ({ zoomIn = true, amount = 0.1 }) => {
    const multiplier = zoomIn ? 1 : -1;
    scale = scale + amount * multiplier;
  };
</script>

<style>
  cardEditor {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .card-preview {
    display: flex;
    padding: 1rem;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .editor-tools {
    flex: 1;
    background: white;
    display: flex;
    flex-direction: column;
    max-width: 40rem;
  }

  .card-ability-title {
    background: var(--primary-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .card-ability-title > button {
    padding: 0.75rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card-basic {
    padding: 1rem;
    display: flex;
    flex: 1;
    flex-direction: row;
  }

  .card-info {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }

  .form-el {
    flex: 0.5;
  }

  .ability-list-container {
    overflow-y: auto;
    padding: 1rem;
  }

  h3 {
    background: var(--primary-color);
    padding: 0.75rem;
    margin: 0;
  }

  /* .card-zoom-settings {
    background: white;
    margin-top: 1rem;
    padding: 0.5rem 1rem;
    border-radius: 1rem;
    bottom: 1rem;
  }

  .card-zoom-settings > button {
    padding: 0.5rem;
    border: 1px solid black;
    font-size: 0.75rem;
    line-height: 0;
    text-align: center;
  } */

  .card-buttons {
    flex: 1;
    justify-content: space-between;
    display: flex;
    align-items: flex-end;
    padding: 1rem;
  }

  .card-buttons > button {
    font-size: 1rem;
    width: 10rem;
  }

  @media print {
    .editor-tools {
      display: none;
    }

    .card-zoom-settings {
      display: none;
    }
  }
</style>

<cardEditor>

  <div class="editor-tools">
    <div>
      <h3>Basic info</h3>
      <div class="card-basic">

        <div class="card-info">
          <div class="form-el">
            <label>Type</label>
            <select bind:value={card.type}>
              {#each cardTypes as typeOption}
                <option value={typeOption.id}>{typeOption.text}</option>
              {/each}
            </select>
          </div>
          <div class="form-el">
            <label>Name</label>
            <input bind:value={card.name} />
          </div>
          <div class="form-el">
            <label>{card.type === 'item' ? 'Mana cost' : 'Health'}</label>
            <input bind:value={card.cost} />
          </div>
          <div class="form-el">
            <label>Card image</label>
            <input type="url" bind:value={card.backgroundImage} />
          </div>
          {#if card.type === 'creature'}
            <div class="form-el">
              <label>Earned XP</label>
              <input type="text" bind:value={card.xp} />
            </div>
          {/if}
          <div class="form-el">
            <label>
              {card.type === 'creature' ? 'Gold reward' : 'Gold cost'}
            </label>
            <input type="text" bind:value={card.gold} />
          </div>
        </div>

        <div class="card-preview">
            <Card {...{ card, scale }} />
            <!-- <div class="card-zoom-settings">
      <button class="icon-button" on:click={() => zoom({ zoomIn: true })}>
        <Icon icon={faPlus} />
      </button>
      <span class="zoom-percentage">{scalePercentage}</span>
      <button class="icon-button" on:click={() => zoom({ zoomIn: false })}>
        <Icon icon={faMinus} />
      </button>
    </div> -->
        </div>

      </div>
    </div>
    <div class="card-abilities">
      <div class="card-ability-title">
        <h3>Abilities</h3>
        <button
          class="icon-button"
          on:click={() => {
            card.abilities.push({});
            card.abilities = card.abilities;
          }}>
          Add new &nbsp;
          <Icon icon={faPlus} />
        </button>
      </div>
    </div>
    <div class="ability-list-container">
      <AbilityList bind:abilities={card.abilities} />
    </div>
    <div class="card-buttons">
      <button on:click={() => dispatch('save', card)}>Save</button>
      <button on:click={() => dispatch('cancel')}>Cancel</button>
    </div>
  </div>

</cardEditor>
