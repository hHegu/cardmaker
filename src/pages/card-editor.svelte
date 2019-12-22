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

  export let card;

  const cardTypes = [
    { id: "creature", text: "Creature" },
    { id: "item", text: "Item" }
  ];
</script>

<style>
  cardEditor {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .card-preview {
    display: flex;
    padding-left: 1rem;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .editor-tools {
    height: 100%;
    background: white;
    display: flex;
    flex-direction: column;
    width: 30rem;
    box-shadow: var(--material-shadow);
  }

  .card-ability-title {
    padding-top: 0.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card-ability-title > button {
    padding: 0.5rem 2rem;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card-basic {
    padding: 1rem 1rem 0.5rem 1rem;
    display: flex;
    flex-direction: row;
  }

  .card-info {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    flex: 1;
  }

  .form-el {
    padding-bottom: 0.5rem;
  }

  select {
    width: 100%;
  }

  input {
    width: 100%;
  }

  .ability-list-container {
    overflow-y: auto;
    flex: 1;
    padding: 0.5rem 1rem 0.5rem 1rem;
  }

  .editor-heading {
    background: var(--primary-color);
    font-weight: 700;
    padding: 0.5rem 1rem;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .card-buttons {
    justify-content: space-between;
    display: flex;
    align-items: flex-end;
  }

  .card-buttons > button {
    margin-left: 0.5rem;
  }

  .new-card-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 1;
    flex-direction: column;
  }

  .new-card-container > button {
    border-radius: 50%;
    padding: 1.5rem;
    line-height: 0;
    font-size: 1.5rem;
  }

  .new-card-container > p {
    font-weight: 500;
  }

  @media print {
    .editor-tools {
      display: none;
    }
  }
</style>

<cardEditor>
  <div class="editor-tools">
    {#if card}
      <div class="editor-heading">
        Card settings
        <div class="card-buttons">
          <button on:click={() => dispatch('save', card)}>Save</button>
          <button on:click={() => dispatch('cancel')}>Cancel</button>
        </div>
      </div>
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
          <Card {card} />
        </div>
      </div>
      <div class="ability-list-container">
        <AbilityList bind:abilities={card.abilities} />
        <div class="card-ability-title">
          <button
            class="icon-button"
            on:click={() => {
              card.abilities.push({});
              card.abilities = card.abilities;
            }}>
            Add ability &nbsp;
            <Icon icon={faPlus} />
          </button>
        </div>
      </div>
    {:else}
      <div class="new-card-container">
        <p>Add new card</p>
        <button on:click={() => dispatch('new')}>
          <Icon icon={faPlus} />
        </button>
      </div>
    {/if}
  </div>
</cardEditor>
