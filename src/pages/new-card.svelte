<script>
  import { createEventDispatcher } from "svelte";
  import CardEditor from "../components/card-editor.svelte"
  const dispatch = createEventDispatcher();
</script>

<style>
  newcard {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }
</style>

<newcard>
  <CardEditor />
</newcard>
