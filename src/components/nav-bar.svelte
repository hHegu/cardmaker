<script>
  import { createEventDispatcher } from "svelte";
  export let currentPage;
  const dispatch = createEventDispatcher();

  const changePage = page => dispatch("changePage", page);
</script>

<style>
  nav-bar {
    padding: 1rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    background: var(--primary-color);
  }

  .title {
    font-weight: bold;
    text-decoration: solid;
    padding-right: 3rem;
    text-transform: uppercase;
    cursor: pointer;
  }

  .tab {
    margin-right: 1rem;
  }

  @media print {
    nav-bar {
      display: none;
    }
  }
</style>

<nav-bar>
  <div class="title" on:click={() => changePage('lander')}>Cardmaker</div>
  <button
    class="tab"
    disabled={currentPage === 'default-cards'}
    on:click={() => changePage('default-cards')}>
    Default cards
  </button>
  <button
    class="tab"
    disabled={currentPage === 'card-editor'}
    on:click={() => changePage('card-editor')}>
    Custom cards
  </button>
</nav-bar>
