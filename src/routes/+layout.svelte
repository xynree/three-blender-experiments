<script lang="ts">
  const exercises = [
    { id: "1-rounded-box", name: "Rounded Box" },
    { id: "2-table", name: "Table" },
    { id: "3-mug", name: "Mug" },
  ];
</script>

<div style="display:flex; height:100vh">
  <nav>
    {#each exercises as ex}
      <a href={`/exercises/${ex.id}`} style="display:block; margin:0.5rem 0">
        {ex.name}
      </a>
    {/each}
  </nav>
  <main>
    <slot />
  </main>
</div>

<style>
  nav {
    width: 200px;
    border-right: 1px solid #ddd;
    padding: 1rem;
  }
  main {
    flex: 1;
    overflow: hidden;
    padding: 1rem;
  }
</style>
