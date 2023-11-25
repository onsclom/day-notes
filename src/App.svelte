<script lang="ts">
  const { demo = false } = $props<{ demo: boolean }>();

  type Mode = "reading" | "writing";
  let mode = $state<Mode>("writing");

  function dateToNiceString(date: Date) {
    return date
      .toLocaleDateString("en-US", {
        weekday: "short",
        month: "short",
        day: "numeric",
        year: "numeric",
      })
      .toLocaleLowerCase();
  }

  // get todays date as Mon D, YYYY
  const today = new Date();
  const date = dateToNiceString(today);

  const now = new Date();

  type Note = { date: string; content: string };

  let notes = [
    {
      date: now,
      content: `take out the trash
      
password: hunter2

desk lenght 1.5m`,
    },
  ];

  if (demo) {
    // lets generate some demo data
  }
</script>

<main>
  <div>
    <span>
      {date}
    </span>
    <button
      onclick={() => {
        mode = mode === "reading" ? "writing" : "reading";
      }}
    >
      {mode === "reading" ? "write" : "read"}
    </button>
  </div>

  {#if mode === "reading"}
    {#each notes as note (note.date)}
      <h2>{date}</h2>
      <pre>{note.content}</pre>
    {/each}
  {:else}
    <textarea placeholder="write here..."></textarea>
  {/if}
</main>

<style>
  main {
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  main > textarea {
    flex: 1;
    height: 100%;
    border: 0;
    box-sizing: border-box;
    padding: 0.5rem;
    resize: none;
  }

  main > div {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem;
  }
</style>
