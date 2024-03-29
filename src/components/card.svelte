<script>
  import _ from "lodash";

  import Icon from "svelte-awesome";

  import { faBolt } from "@fortawesome/free-solid-svg-icons/faBolt";
  import { faCoins } from "@fortawesome/free-solid-svg-icons/faCoins";
  import { faHeart } from "@fortawesome/free-solid-svg-icons/faHeart";
  import { faBoxOpen } from "@fortawesome/free-solid-svg-icons/faBoxOpen";
  import { faDragon } from "@fortawesome/free-solid-svg-icons/faDragon";
  import { faCircle } from "@fortawesome/free-solid-svg-icons/faCircle";
  import { faTimes } from "@fortawesome/free-solid-svg-icons/faTimes";
  import { faUser } from "@fortawesome/free-solid-svg-icons/faUser";
  import { faStar } from "@fortawesome/free-solid-svg-icons/faStar";

  import OneStack from "../icons/oneStack.svelte";
  import TwoStack from "../icons/twoStack.svelte";
  import ThreeStack from "../icons/threeStack.svelte";
  import { ABILITY_CONFIG, CARD_COLORS } from "../util/constants.js";

  const CARD_ICONS = {
    creature: { type: faDragon, cost: faHeart },
    item: { type: faBoxOpen, cost: faBolt },
    skill: { type: faStar, cost: faBolt },
    character: { type: faUser, cost: faHeart }
  };

  const getCardAmountIcon = amount => {
    switch (Number(amount)) {
      case 1:
        return OneStack;
      case 2:
        return TwoStack;
      case 3:
        return ThreeStack;
      default:
        return undefined;
    }
  };

  export let card;
</script>

<style>
  .card {
    width: var(--card-width); /* ~63mm */
    height: var(--card-height); /* ~88mm */
    font-size: 2.5mm;
    background: white;
    display: flex;
    flex-direction: column;
    box-shadow: var(--material-shadow);
  }

  .card-image {
    height: 35mm;
    flex-shrink: 0;
    flex-grow: 0;
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
    border-radius: 0 0.5rem 0 0;
  }
  .card-bottom-right {
    padding: 2mm;
    border-radius: 0.5rem 0 0 0;
  }
  .card-top-left {
    padding: 2mm;
    border-radius: 0 0 0.5rem 0;
    display: flex;
    align-self: center;
    justify-content: center;
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
    background: white;
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
    justify-content: space-between;
    align-items: center;
    padding: 1mm;
    line-height: 0;
    background: white;
  }

  .footer-right > span {
    height: 100%;
    padding-left: 1mm;
  }

  @media print {
    .card {
      box-shadow: none;
      transform: none !important;
    }
  }
</style>

<div class="card">
  <div
    class="card-image"
    style="background-image: url('{card.backgroundImage}');">
    <div class="image-info-group">
      {#if card.type !== 'item'}
        <div class="card-top-left">
          <span style="padding-right: 1mm">{card.cost || '0'}</span>
          <Icon data={CARD_ICONS[card.type].cost} scale="0.7" />
        </div>
      {/if}
    </div>
    <div class="image-info-group">
      <div class="card-bottom-left">{card.name || 'Card name here'}</div>
      <div
        class="card-bottom-right card-type"
        style="color: {card.type === 'skill' ? CARD_COLORS[card.color] : 'white'}">
        <Icon data={CARD_ICONS[card.type].type} />
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
              {#if abilityNumber}
                <span style="font-weight: 500">{abilityNumber}</span>
              {/if}
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
    <div class="footer-left">
      {#if card.type === 'creature'}
        <svelte:component
          this={getCardAmountIcon(card.actionCards.attacks)}
          color={ABILITY_CONFIG.attack.color} />
        <svelte:component
          this={getCardAmountIcon(card.actionCards.defends)}
          color={ABILITY_CONFIG.defend.color} />
        <svelte:component
          this={getCardAmountIcon(card.actionCards.specials)}
          color={ABILITY_CONFIG.special.color} />
      {/if}
    </div>
    <div class="footer-right">
      {#if card.type === 'creature'}
        <span>
          {card.xp}
          <b>XP</b>
        </span>
      {/if}
      <span>
        {card.gold}
        <Icon data={faCoins} scale="0.6" />
      </span>
    </div>
  </div>
</div>
