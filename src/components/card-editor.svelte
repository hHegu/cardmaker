<script>
  import _ from "lodash";

  import Icon from "fa-svelte";

  import { faTimes } from "@fortawesome/free-solid-svg-icons/faTimes";
  import { faArrowUp } from "@fortawesome/free-solid-svg-icons/faArrowUp";
  import { faArrowDown } from "@fortawesome/free-solid-svg-icons/faArrowDown";
  import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";

  import Card from "../components/card.svelte";
  import AbilityList from "../components/ability-list.svelte";

  let card = {
    cost: undefined,
    name: undefined,
    type: "creature",
    backgroundImage: undefined,
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

  const cardTypes = [
    { id: "creature", text: "Creature" },
    { id: "item", text: "Item" }
  ];
</script>

<style>
  cardEditor {
    display: flex;
    width: 100%;
    height: 100%;
  }

  .card {
    display: flex;
    flex: 1;
    justify-content: center;
    align-items: center;
  }

  .editor-tools {
    flex: 1;
    background: white;
    display: flex;
    flex-direction: column;
  }

  button {
    background: none;
    box-shadow: none;
    padding: 0;
  }

  .card-ability-title {
    background: var(--primary-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .card-ability-title>button {
    padding: 0.75rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card-basic {
    padding: 1rem;
    display: flex;
    flex: 1;
    flex-direction: column;
  }

  .form-row {
    display: flex;
    flex: 1;
    flex-direction: row;
  }

  .form-el {
    padding-bottom: 0.5rem;
    padding-right: 1rem;
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
</style>

<cardEditor>
  <div class="card">
    <Card {card} />
  </div>
  <div class="editor-tools">
    <div>
      <h3>Basic info</h3>
      <div class="card-basic">

        <div class="form-row">
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
        </div>

        <div class="form-row">
          <div class="form-el">
            <label>Card image</label>
            <input type="url" bind:value={card.backgroundImage} />
          </div>
          {#if card.type === 'creature'}
            <div class="form-el">
              <label>Earned XP</label>
              <input type="text" bind:value={card.experience} />
            </div>
          {/if}
          <div class="form-el">
            <label>
              {card.type === 'creature' ? 'Gold reward' : 'Gold cost'}
            </label>
            <input type="text" bind:value={card.gold} />
          </div>
        </div>

      </div>
    </div>
    <div class="card-abilities">
      <div class="card-ability-title">
        <h3>Abilities</h3>
        <button
          on:click={() => {
            card.abilities.push({});
            card.abilities = card.abilities;
          }}>
          Add new 
          <Icon icon={faPlus} />
        </button>
      </div>
    </div>
    <div class="ability-list-container">
      <AbilityList bind:abilities={card.abilities} />
    </div>
  </div>
</cardEditor>
