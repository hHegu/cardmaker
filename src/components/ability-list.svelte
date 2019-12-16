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

  button {
    background: none;
    box-shadow: none;
    padding: 0;
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

  textarea {
    width: 20rem;
    font-size: 0.75rem;
  }

  .ability-description-container {
    padding-top: 0.5rem;
  }

  .ability-header {
    display: flex;
    justify-content: space-between;
  }
</style>

<abilityList>
  {#each abilities as { type, abilityNumber, description }, i}
    <div class="ability">
      <div class="ability-header">
        <h5>Ability {i + 1}</h5>
        <div>
          {#if i < abilities.length - 1}
            <button
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
      </div>
      <div class="ability-description-container">
        <label>Description</label>
        <textarea bind:value={description} />
      </div>
    </div>
  {/each}
</abilityList>
