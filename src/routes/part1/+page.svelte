<script lang="ts">
  import part1Forecast from "$lib/data/part1_forecast.json";

  let activeSection = 0;
  let isClicking = false; // To temporarily disable scroll logic

  const handleClick = (index: number) => {
    isClicking = true; // Temporarily disable scroll logic
    activeSection = index;

    const section = document.querySelector(`#section-${index}`);
    if (section && section instanceof HTMLElement) {
      // Type guard to check it's an HTMLElement
      // Adjust scroll position with an offset
      window.scrollTo({
        top: section.offsetTop - 100, // Adjust 80px to the height of your sticky navbar
        behavior: "smooth",
      });
    }

    // Re-enable scroll handling after scrolling finishes
    setTimeout(() => {
      isClicking = false;
    }, 2000); // Adjust timeout based on scrolling duration
  };
</script>

<div class="my-10">
  <h1 class="text-center font-semibold text-xl sm:text-4xl mb-5 sm:mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 1!
  </h1>

  <div class="sticky top-0 bg-white">
    <div class="flex justify-center mb-5 pt-3 space-x-5">
      <a href="/" class="btn btn-sm sm:btn-md btn-outline btn-primary">Giới thiệu</a>
      <a href="/part1" class="btn btn-sm sm:btn-md btn-primary">Part1</a>
      <a href="/part2-3" class="btn btn-sm sm:btn-md btn-outline btn-primary">Part 2&3</a>
    </div>

    <div
      class="flex overflow-x-auto whitespace-nowrap space-x-6 border-b-2 border-gray-200 mx-5 sm:mx-20"
    >
      {#each part1Forecast as item, i}
        <a
          href="#section-{i}"
          on:click={(e) => {
            e.preventDefault(); // Prevent default anchor behavior
            handleClick(i);
          }}
          class="flex-shrink-0 text-md py-3 px-4 cursor-pointer hover:text-primary transition-colors duration-300"
          class:border-b-2={activeSection === i}
          class:border-black={activeSection === i}
          class:text-primary={activeSection === i}
        >
          {item.title}
        </a>
      {/each}
    </div>
  </div>

  <div class="px-10 sm:px-20 flex justify-center">
    <div>
      {#each part1Forecast as item, i}
        <div id="section-{i}" class="h-5 sm:h-10"></div>
        <div
          class="text-lg sm:text-xl flex font-medium mt-5 mb-5 border border-base-300 rounded-lg px-2 sm:px-4 py-3 shadow-sm hover:shadow-md
          transition-shadow duration-300 bg-primary-content space-x-3 sm:space-x-7"
        >
          <img src="/images/title.png" alt="Title" class="w-9 h-9" />
          <span class="mt-1">{item.title}</span>
        </div>

        <div class="mb-2 space-y-5 ml-5 sm:ml-10">
          {#each item.questions as question}
            <div class="flex space-x-2">
              <img src="/images/question.png" alt="Question" class="w-7 h-7" />
              <span>{question}</span>
            </div>
          {/each}
        </div>
      {/each}
    </div>
  </div>
</div>
