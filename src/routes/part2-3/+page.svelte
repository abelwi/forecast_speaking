<script lang="ts">
  import part23Forecast from "$lib/data/part23_forecast.json";

  let activeSection = 0;
  let isClicking = false; // To temporarily disable scroll logic

  const handleClick = (index: number) => {
    isClicking = true; // Temporarily disable scroll logic
    activeSection = index;

    const section = document.querySelector(`#section-${index}`);
    if (section && section instanceof HTMLElement) { // Type guard to check it's an HTMLElement
      // Adjust scroll position with an offset
      window.scrollTo({
        top: section.offsetTop - 150, // Adjust 80px to the height of your sticky navbar
        behavior: "smooth",
      });
    }

    // Re-enable scroll handling after scrolling finishes
    setTimeout(() => {
      isClicking = false;
    }, 2000); // Adjust timeout based on scrolling duration
  };
</script>

<div class="my-10 mx-20">
  <h1 class="text-center font-semibold text-4xl mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 2&3 <em class="text-primary">tháng 12</em>!
  </h1>

  <div class="sticky top-0 bg-white">
    <div class="flex">
      <div class="flex w-1/2 mb-5 pt-3 space-x-5">
        <a href="/" class="btn btn-outline btn-primary">Giới thiệu</a>
        <a href="/part1" class="btn btn-outline btn-primary">Part1</a>
        <a href="/part2-3" class="btn btn-primary">Part 2&3</a>
      </div>

      <div class="flex justify-center w-1/2 overflow-x-auto whitespace-nowrap space-x-16 border-2 border-gray-200 py-3 mt-3 rounded-lg">
        {#each part23Forecast as topic, i}
          <a href="#section-{i}"
          on:click={(e) => {
            e.preventDefault(); // Prevent default anchor behavior
            handleClick(i);
          }}
          class="flex-shrink-0 text-md py-2 px-4 cursor-pointer hover:text-primary transition-colors duration-300"
          class:border-b-2={activeSection === i}
          class:border-black={activeSection === i}
          class:text-primary={activeSection === i}
          >
            {topic.topic}
          </a>
        {/each}
      </div>
    </div>

    <div class="flex overflow-x-auto whitespace-nowrap border-b-2 border-gray-200 text-lg font-semibold mt-8 pb-3">
      <p class="w-1/2 text-center">Part 2</p>
      <p class="w-1/2 text-center">Part 3</p>
    </div>
    
  </div>

  {#each part23Forecast as topic, i}
    {#if i > 0}
      <div class="divider"></div>
    {/if}
    <div id="section-{i}" class="h-5"></div>
    <h1 class="text-2xl mt-5 font-semibold">{topic.topic}</h1>
    {#each topic.titles as item, i}
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
  {/each}
</div>


