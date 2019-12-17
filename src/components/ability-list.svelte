<script>
  export let abilities;
  import _ from "lodash";

  import Icon from "fa-svelte";

  import { faTimes } from "@fortawesome/free-solid-svg-icons/faTimes";
  import { faArrowUp } from "@fortawesome/free-solid-svg-icons/faArrowUp";
  import { faArrowDown } from "@fortawesome/free-solid-svg-icons/faArrowDown";
  import { faPlus } from "@fortawesome/free-solid-svg-icons/faPlus";

  const abilityTypes = [
    { id: undefined, text: "None" },
    { id: "attack", text: "Attack" },
    { id: "defend", text: "Defend" },
    { id: "special", text: "Special" }
  ];

  const moveArrayElement = ({ array, index, moveUp }) => {
    const tempArray = _.clone(array);
    const nextIndex = index + (moveUp ? 1 : -1);
    let tempValue;

    tempValue = tempArray[index];
    tempArray[index] = tempArray[nextIndex];
    tempArray[nextIndex] = tempValue;
    return tempArray;
  };
</script>

<style>
  h5 {
    margin: 0 0 0.5rem 0;
  }

  .ability {
    padding: 0.5rem;
    margin-bottom: 0.5rem;
    box-shadow: 1px 1px 3px 0px #00000066;
  }

  .type-and-number {
    display: flex;
    flex-direction: row;
  }

  .ability-description {
    flex: 1;
    padding-left: 0.5rem;
  }

  textarea {
    width: 100%;
    font-size: 0.75rem;
    padding: 0.5rem;
  }

  .ability-header {
    display: flex;
    justify-content: space-between;
  }
</style>

<abilityList>
  {#each abilities as { type, abilityNumber, description }, i}
    <div class="ability" style={i >= abilities.length - 1 ? 'margin: 0' : ''}>
      <div class="ability-header">
        <h5>Ability {i + 1}</h5>
        <div>
          {#if i < abilities.length - 1}
            <button
              class="icon-button"
              on:click={() => {
                abilities = moveArrayElement({
                  array: abilities,
                  index: i,
                  moveUp: true
                });
              }}>
              <Icon icon={faArrowDown} />
            </button>
          {/if}

          {#if i > 0}
            <button
              class="icon-button"
              on:click={() => {
                if (i > 0) {
                  abilities = moveArrayElement({
                    array: abilities,
                    index: i,
                    moveUp: false
                  });
                }
              }}>
              <Icon icon={faArrowUp} />
            </button>
          {/if}

          <button
            class="icon-button"
            on:click={() => {
              abilities.splice(i, 1);
              abilities = abilities;
            }}>
            <Icon icon={faTimes} />
          </button>
        </div>
      </div>
      <div class="type-and-number">
        <div>
          <label>Type</label>
          <select bind:value={type}>
            {#each abilityTypes as typeOption}
              <option value={typeOption.id}>{typeOption.text}</option>
            {/each}
          </select>
        </div>
        <div style="padding-left: 0.5rem">
          <label>Nmbr</label>
          <select bind:value={abilityNumber}>
            {#each [1, 2, 3] as n}
              <option value={n}>{n}</option>
            {/each}
          </select>
        </div>
        <div class="ability-description">
          <label>Description</label>
          <textarea rows="3" bind:value={description} />
        </div>
      </div>

    </div>
  {/each}
</abilityList>
