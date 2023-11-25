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

  const today = new Date();
  const date = dateToNiceString(today);
  const now = new Date();

  type Note = { date: string; content: string };

  let notes = $state<Note[]>([
    {
      date: dateToNiceString(now),
      content: ``.toLocaleLowerCase(),
    },
    {
      date: dateToNiceString(new Date(now.getTime() - 1000 * 60 * 60 * 24)),
      content:
        `Had a productive team meeting this morning, discussing project timelines. Lunch at 'The Green Bowl' was a delight - the salad was fresh and flavorful.

Read more of 'The Alchemist' - it's getting interesting.

Alex's birthday tomorrow, must send a card.

Grateful for: Friends, family, sunny days.`.toLocaleLowerCase(),
    },
    {
      date: dateToNiceString(new Date(now.getTime() - 1000 * 60 * 60 * 24 * 2)),
      content: `Woke up not feeling great, skipped gym.

Finished marketing proposal draft!

Watched a documentary on minimalism. Very inspiring.

Dinner: Made Spaghetti Aglio e Olio. Yum!

Patience is key in life’s challenges.`.toLocaleLowerCase(),
    },
  ]);
  const todaysNote = $derived(notes[0]!);

  function withViewTransition(fn: () => void) {
    // @ts-ignore
    if (!document.startViewTransition) fn();
    // @ts-ignore
    document.startViewTransition(() => {
      fn();
    });
  }
</script>

<main>
  <div class="top-bar">
    <i>
      {mode === "reading" ? `📖 ${notes.length} notes` : date}
    </i>
    <button
      onclick={() =>
        withViewTransition(() => {
          mode = mode === "reading" ? "writing" : "reading";
        })}
    >
      {mode === "reading" ? "✏️ write" : "📖 read"}
    </button>
  </div>

  {#if mode === "reading"}
    <div class="note-container">
      {#each notes as note (note.date)}
        <div>
          <h2>{note.date}</h2>
          <pre>{note.content}</pre>
        </div>
      {/each}
      <i style="text-align: center"
        >you have reached the end of your notes! 🏖️</i
      >
    </div>
  {:else}
    <textarea placeholder="write here..." bind:value={todaysNote.content}
    ></textarea>
  {/if}
</main>

<style>
  html,
  body {
    height: 100%;
  }

  pre {
    font-size: medium;
    white-space: pre-wrap;
  }

  .note-container {
    overflow: auto;
    padding: 0.5rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  main {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  main > textarea {
    display: block;
    flex: 1;
    border: 0;
    box-sizing: border-box;
    padding: 0.5rem;
    resize: none;
    font-size: medium;
  }

  .top-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0.5rem;
    border-bottom: 1px solid #ccc;
  }
</style>
