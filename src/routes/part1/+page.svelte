<script lang="ts">
  import topicForecast from "$lib/data/topic_forecast.json";
</script>

{#each topicForecast as topic}
  <h1>{topic.topic}</h1>
  
  {#each topic.titles as item}
    <p>{item.title}</p>

    {#each item.cueCards as cueCard}
      <li>{cueCard}</li>
    {/each}

    {#each item.questions as question}
      <p><strong>{question}</strong></p>
    {/each}
  {/each}
{/each}