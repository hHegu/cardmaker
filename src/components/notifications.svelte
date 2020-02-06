<script>
  import _ from "lodash";
  import { fly } from 'svelte/transition';

  import Icon from "svelte-awesome";
  import { faExclamationCircle } from "@fortawesome/free-solid-svg-icons/faExclamationCircle";
  import { faInfoCircle } from "@fortawesome/free-solid-svg-icons/faInfoCircle";

  export let notifications = [];
</script>

<style>
  notifications {
    position: fixed;
    left: 0.5rem;
    bottom: 0.5rem;
  }

  .notification {
    display: flex;
    padding: 0.5rem 1rem;
    min-width: 30rem;
    max-width: 60rem;
    align-items: center;
    background: var(--light-color);
    border-radius: 1rem;
    margin-top: 0.5rem;
  }

  .error {
    background: #f44336;
  }

  .message {
    padding-left: 0.5rem;
  }
</style>

<notifications>
  {#each notifications as notification}
    <div
      transition:fly="{{ y: 200, duration: 300 }}"
      class={`notification ${notification.isError ? 'error' : ''}`}>
      <Icon data={notification.isError ? faExclamationCircle : faInfoCircle} />
      <span class="message">{notification.message}</span>
    </div>
  {/each}
</notifications>
