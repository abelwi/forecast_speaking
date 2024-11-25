<script lang="ts">
  import part1Forecast from "$lib/data/part1_forecast.json";
</script>

{#each part1Forecast as item}
  <div>
    <h2 class="text-lg font-medium">
      {item.title}
    </h2>
    {#each item.questions as question}
      {question}
    {/each}
  </div>
{/each}
