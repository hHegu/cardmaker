<script>
  import { CARD_TYPE_COLORS } from "../util/constants.js";
  export let type = "";
</script>

<style>
  .card-type-tag-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @media print {
    .card-type-tag-container {
      display: none;
    }
  }

  .card-type-tag {
    background: var(--primary-color);
    padding: 0.25rem;
    flex: 1;
    text-align: center;
    border-radius: 0.5rem 0.5rem 0 0;
    font-weight: bolder;
    font-size: small;
  }
</style>

<div class="card-type-tag-container">
  <div class="card-type-tag" style={`background: ${CARD_TYPE_COLORS[type]}`}>
    {type}
  </div>
</div>
