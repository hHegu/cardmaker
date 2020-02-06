<script>
  import _ from "lodash";

  import { createEventDispatcher } from "svelte";

  import Icon from "svelte-awesome";
  import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";

  import Card from "../components/card.svelte";
  import AbilityList from "../components/ability-list.svelte";

  import * as firebase from "firebase";

  const dispatch = createEventDispatcher();
  export let card;

  const cardTypes = [
    { id: "creature", text: "Creature" },
    { id: "character", text: "Character" },
    { id: "item", text: "Item" },
    { id: "skill", text: "Skill" }
  ];

  const cardColors = [
    { id: null, text: "Unset" },
    { id: "red", text: "Red" },
    { id: "blue", text: "Blue" },
    { id: "yellow", text: "Yellow" }
  ];

  const prepareFileForSending = (files) => {
    const file = _.get(files, 'target.files.0')
    if (file) {
      dispatch('saveImage', {card, file})
    }
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

  .card-ability-title > button:hover {
    background: #dddddd;
    color: black;
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

  .action-cards {
    display: flex;
    flex-direction: row;
  }
  .action-cards > div {
    flex: 1;
  }

  label {
    font-size: 0.75rem;
  }

  select {
    margin-top: 3px;
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
            {#if card.type !== 'item'}
              <label>{card.type === 'skill' ? 'Mana cost' : 'Health'}</label>
              <input bind:value={card.cost} />
            {/if}
          </div>
          <div class="form-el">
            <label>Card image</label>
            <input
              type="file"
              accept="image/png, image/jpeg"
              on:change={prepareFileForSending} />
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
          {#if card.type === 'skill'}
            <div class="form-el">
              <label>Card color</label>
              <select bind:value={card.color}>
                {#each cardColors as colorOption}
                  <option value={colorOption.id}>{colorOption.text}</option>
                {/each}
              </select>
            </div>
          {/if}
          {#if card.type === 'creature'}
            <div class="form-el action-cards">
              <div>
                <label>Attacks</label>
                <select bind:value={card.actionCards.attacks}>
                  {#each [0, 1, 2, 3] as n}
                    <option value={n}>{n}</option>
                  {/each}
                </select>
              </div>
              <div>
                <label>Defends</label>
                <select bind:value={card.actionCards.defends}>
                  {#each [0, 1, 2, 3] as n}
                    <option value={n}>{n}</option>
                  {/each}
                </select>
              </div>
              <div>
                <label>Specials</label>
                <select bind:value={card.actionCards.specials}>
                  {#each [0, 1, 2, 3] as n}
                    <option value={n}>{n}</option>
                  {/each}
                </select>
              </div>
            </div>
          {/if}
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
            <Icon data={faPlus} />
          </button>
        </div>
      </div>
    {:else}
      <div class="new-card-container">
        <p>Add new card</p>
        <button on:click={() => dispatch('new')}>
          <Icon data={faPlus} />
        </button>
      </div>
    {/if}
  </div>
</cardEditor>
