<script lang="ts">
  import { fly, fade } from "svelte/transition";
  import { cubicOut } from "svelte/easing";

  let santasCounter = $state(0);

  function handleSubmit(
    e: SubmitEvent & { currentTarget: EventTarget & HTMLFormElement }
  ): any {
    e.preventDefault();
    santasCounter++;
  }

  function handleReset(
    e: Event & { currentTarget: EventTarget & HTMLFormElement }
  ): any {
    e.preventDefault();
    santasCounter = 0;
  }
</script>

<div>
  {#key santasCounter}
    <h1 in:fly={{ x: 20, duration: 200 }}>
      {santasCounter}
    </h1>
  {/key}
</div>

<form onsubmit={(e) => handleSubmit(e)} onreset={(e) => handleReset(e)}>
  <button type="submit">Add</button>
  <button type="reset">Reset</button>
</form>

<style>
</style>
