<script lang="ts">
  import part23Forecast from "$lib/data/part23_forecast.json";
</script>

<div class="my-10 mx-20">
  <h1 class="text-center font-semibold text-4xl mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 2&3!
  </h1>

  <div class="flex overflow-x-auto whitespace-nowrap border-b-2 border-gray-200 py-5 mb-5 sticky top-0 bg-white">
    <p class="w-1/2 text-center">Part2</p>
    <p class="w-1/2 text-center">Part3</p>
  </div>
  {#each part23Forecast as item, i}
    {#if i > 0}
      <div class="divider"></div>
    {/if}

    <div class="flex items-center space-x-10">
      <div class="flex space-x-5 w-1/2 bg-primary-content text-lg text-center px-5 pt-3 pb-6 rounded-lg">
        <img src="src/images/describe.png" alt="Describe" class="w-16 h-16">
        <span class="mt-4">{item.title}</span>
      </div>

      <div class="w-1/2 mt-5">
        {#each item.questions as question}
          <p class="flex space-x-5 border border-primary-content px-4 py-2 rounded-lg mb-5">
            <img
                src="src/images/question.png"
                alt="Question"
                class="w-7 h-7"
              />
            <span>{question}</span>
          </p>
        {/each}
      </div>
    </div>
  {/each}
</div>