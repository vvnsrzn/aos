<script lang="ts">
  import { onMount } from "svelte";
  let inputName: HTMLInputElement;

  type Child = { name: string; tally: number };
  let childList: Child[] = $state([]);
  onMount(async () => {
    inputName.focus();
    const res = await fetch(
      "https://advent.sveltesociety.dev/data/2023/day-one.json"
    );
    childList = await res.json();
  });

  function handleSubmit(
    e: SubmitEvent & { currentTarget: EventTarget & HTMLFormElement }
  ) {
    e.preventDefault();
    const child: Child = { name: "", tally: NaN };
    const form = new FormData(e.currentTarget);
    for (const [k, v] of form.entries()) {
      if (k === "name") child.name = v.toString();
      if (k === "tally") child.tally = +v;
    }
    childList.unshift(child);
    e.currentTarget.reset();
    inputName.focus();
  }
</script>

<form onsubmit={(e) => handleSubmit(e)}>
  <input
    type="text"
    name="name"
    placeholder="Fill the name"
    bind:this={inputName}
  />
  <input type="number" name="tally" placeholder="Fill the tally" />
  <button type="submit">Submit</button>
</form>

{#each childList as child, i}
  <p class={child.tally > 0 ? "kind" : "naughty"}>
    {child.name}
    {child.tally}
  </p>
{/each}

<style>
  .kind {
    color: rebeccapurple;
  }
  .naughty {
    color: red;
    text-decoration: line-through;
  }
</style>
