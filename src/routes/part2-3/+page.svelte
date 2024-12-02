<script lang="ts">
  import part23Forecast from "$lib/data/part23_forecast.json";
</script>

<div class="my-10 mx-20">
  <h1 class="text-center font-semibold text-4xl mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 2&3!
  </h1>

  <div class="flex justify-center mb-10 space-x-5">
    <a href="/" class="btn btn-outline btn-primary">Part1</a>
    <a href="/part2-3" class="btn btn-primary">Part 2&3</a>
  </div>

  <div class="flex overflow-x-auto whitespace-nowrap border-b-2 border-gray-200 text-lg font-semibold py-5 mb-5 sticky top-0 bg-white">
    <p class="w-1/2 text-center">Part 2</p>
    <p class="w-1/2 text-center">Part 3</p>
  </div>
  {#each part23Forecast as item, i}
    {#if i > 0}
      <div class="divider"></div>
    {/if}

    <div class="flex items-center space-x-10">
      <div class="w-1/2">
        <div class="flex items-center space-x-7 bg-primary-content text-lg text-center px-5 py-4 rounded-lg">
          <img src="src/images/describe.png" alt="Describe" class="w-16 h-16">
          <span>{item.title}</span>
        </div>
        <div class="mt-5">
          <p class="font-semibold mb-2"><em>Cue Card:</em></p>
          {#each item.cueCards as cueCart}
            <li class="ml-5">{cueCart}</li>
          {/each}
        </div>
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


