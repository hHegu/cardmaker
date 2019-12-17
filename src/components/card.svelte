<script>
  import _ from "lodash";

  import Icon from "fa-svelte";

  import { faBolt } from "@fortawesome/free-solid-svg-icons/faBolt";
  import { faCoins } from "@fortawesome/free-solid-svg-icons/faCoins";
  import { faHeart } from "@fortawesome/free-solid-svg-icons/faHeart";
  import { faBoxOpen } from "@fortawesome/free-solid-svg-icons/faBoxOpen";
  import { faDragon } from "@fortawesome/free-solid-svg-icons/faDragon";

  const ABILITY_CONFIG = {
    attack: {
      color: "#f44336",
      icon:
        "https://vectr.com/hhegu/bI7HJ2fw.svg?width=130&height=130&select=bbHS490oX"
    },
    defend: {
      color: "#64b5f6",
      icon:
        "https://vectr.com/hhegu/bI7HJ2fw.svg?width=130&height=130&select=b6lgrj3lp"
    },
    special: {
      color: "#fbc02d",
      icon:
        "https://vectr.com/hhegu/bI7HJ2fw.svg?width=130&height=130&select=c7nTglNvy"
    }
  };

  const CARD_ICONS = {
    creature: { type: faDragon, cost: faHeart },
    item: { type: faBoxOpen, cost: faBolt }
  };

  export let card;
  export let scale;
</script>

<style>
  .card {
    width: 2.5in; /* ~63mm */
    height: 3.5in; /* ~88mm */
    font-size: 2.5mm;
    background: white;
    display: flex;
    flex-direction: column;
    box-shadow: 7px 7px 8px 0px #4e4e4e;
  }

  .card-image {
    height: 35mm;
    flex-shrink: 0;
    flex-grow: 0;
    background: aliceblue;
    display: flex;
    flex-direction: column;

    justify-content: space-between;
    background-size: cover;
  }

  .image-info-group > * {
    background: #0000004a;
    color: white;
    text-shadow: 1px 1px 2px #4e4e4e;
  }

  .image-info-group {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .card-bottom-left {
    padding: 2mm;
    border-radius: 0 1rem 0 0;
  }
  .card-bottom-right {
    padding: 2mm;
    border-radius: 1rem 0 0 0;
  }
  .card-top-left {
    padding: 2mm;
    border-radius: 0 0 1rem 0;
  }

  .card-type {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 3mm;
    font-size: 4mm;
  }

  .card-abilities-container {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
  }

  .ability {
    display: flex;
    align-items: center;
    padding: 1mm 2mm;
    flex: 1;
  }

  .ability-type {
    font-weight: 800;
    margin: 0;
    flex: 0.3;
    text-align: center;
  }

  .ability-description {
    flex: 1;
    margin: 0;
  }

  .ability-icon-container {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1mm 2mm;
    border-radius: 3mm;
    width: 8mm;
    height: 4mm;
    color: #333;
  }

  .ability-icon-container > img {
    height: 4mm;
    width: 4mm;
    flex: 1;
  }

  .ability-icon-container > span {
    flex: 1;
    font-weight: 700;
    line-height: 0;
  }

  .card-footer {
    height: 3mm;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    padding: 1mm;
  }

  .card-footer > span {
    height: 100%;
    padding-left: 1mm;
  }

  @media print {
    .card {
      box-shadow: none;
      border: 1px solid #4e4e4e;
      transform: none !important;
    }
  }
</style>

<div class="card" style={scale ? `transform: scale(${scale})` : ''}>
  <div
    class="card-image"
    style="background-image: url('{card.backgroundImage}');">
    <div class="image-info-group">
      <div class="card-top-left">
        {card.cost || '0'}
        <Icon icon={CARD_ICONS[card.type].cost} />
      </div>
    </div>
    <div class="image-info-group">
      <div class="card-bottom-left">{card.name || 'Card name here'}</div>
      <div class="card-bottom-right card-type">
        <Icon icon={CARD_ICONS[card.type].type} />
      </div>
    </div>
  </div>
  <div class="card-abilities-container card-element">
    {#each card.abilities as { type, abilityNumber, description }}
      <div class="ability">
        {#if type}
          <p class="ability-type" style="color: {ABILITY_CONFIG[type].color}">
            <span
              class="ability-icon-container"
              style="background-color: {ABILITY_CONFIG[type].color}">
              <img src={ABILITY_CONFIG[type].icon} alt="attack" />
              <span>{abilityNumber}</span>
            </span>
          </p>
        {/if}
        <p class="ability-description">
          {@html description}
        </p>
      </div>
    {/each}
  </div>
  <div class="card-footer">
    {#if card.type === 'creature'}
      <span>
        {card.xp}
        <b>XP</b>
      </span>
    {/if}
    <span>
      {card.gold}
      <Icon icon={faCoins} />
    </span>
  </div>
</div>
