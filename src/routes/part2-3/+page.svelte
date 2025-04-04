<script lang="ts">
  import part23Forecast from "$lib/data/part23_forecast.json";

  let activeSection = 0;
  let isClicking = false; 
  let searchTerm = '';
  let filteredData = [];

  // State to manage collapsed/expanded status for each topic
  let collapsed = Array(part23Forecast.length).fill(true);

  // Toggle visibility for a specific topic
  const toggleCollapse = (index: number) => {
    collapsed[index] = !collapsed[index];
  };

  const handleClick = (index: number) => {
    isClicking = true; // Temporarily disable scroll logic
    activeSection = index;

    const section = document.querySelector(`#section-${index}`);
    if (section && section instanceof HTMLElement) {
      window.scrollTo({
        top: section.offsetTop - 180, 
        behavior: "smooth",
      });
    }

    // Re-enable scroll handling after scrolling finishes
    setTimeout(() => {
      isClicking = false;
    }, 2000); 
  };

  // Filter the data whenever the search term changes
  $: filteredData = searchTerm
    ? part23Forecast
        .map(topic => ({
          ...topic,
          titles: topic.titles.filter(title =>
            title.title.toLowerCase().includes(searchTerm.toLowerCase())
          ),
        }))
        .filter(topic => topic.titles.length > 0) 
    : [];
  
  // Handle clicking a suggestion
  const handleSuggestionClick = (title: string) => {
    const normalizedTitle = title.trim().toLowerCase(); // Normalize the search term

    // Find the topic index where the title exists
    const topicIndex = part23Forecast.findIndex(topic =>
      topic.titles.some(t => t.title.trim().toLowerCase() === normalizedTitle)
    );

    if (topicIndex !== -1) {
      // Find the index of the title within the topic
      const titleIndex = part23Forecast[topicIndex].titles.findIndex(t => 
        t.title.trim().toLowerCase() === normalizedTitle
      );

      if (titleIndex !== -1) {
        // Scroll to the exact section (topic + title) within the page
        const section = document.querySelector(`#section-${topicIndex}-${titleIndex}`);
        if (section && section instanceof HTMLElement) {
          window.scrollTo({
            top: section.offsetTop - 180, // Adjust scroll position
            behavior: "smooth"
          });
        }
      }
    }
  };

</script>

<div class="my-14 mx-2 sm:mx-20">
  <h1 class="text-center font-semibold text-xl sm:text-4xl mb-5 sm:mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 2&3!
  </h1>

  <div class="sticky w-full top-0 bg-white">
    <div class="flex justify-between pt-3 mb-0 sm:mb-0">
      <div class="mt-2 sm:mt-0 space-x-2 sm:space-x-5">
        <a href="/part1" class="btn btn-xs sm:btn-md btn-outline btn-primary">Part1</a>
        <a href="/part2-3" class="btn btn-xs sm:btn-md btn-primary">Part 2&3</a>
      </div>

      <div class="relative w-1/2 mb-5">
        <!-- Search Box -->
        <div class="flex text-center">
          <input
            type="text"
            placeholder="Tìm câu hỏi Part 2 ..."
            bind:value={searchTerm}
            class="input input-bordered w-full h-10 sm:h-12"
          />
        </div>

        <!-- Suggestion Box -->
        {#if searchTerm}
          <div class="search-results">
            {#if filteredData.length === 0}
              <p class="text-center text-pink-500 py-3">Không tìm thấy câu nào bạn ơi :)</p>
            {:else}
              <div class="suggestion-list">
                {#each filteredData as topic}
                  {#each topic.titles as title}
                    <button
                      type="button"
                      class="hover:bg-base-300 hover:px-5 py-2 cursor-pointer"
                      on:click={() => {
                        handleSuggestionClick(title.title);
                        searchTerm = '';
                      }}

                    >
                      <li>{title.title}</li>
                    </button>
                  {/each}
                {/each}
              </div>
            {/if}
          </div>
        {/if}
      </div>
    </div>
    
    <div class="flex sm:justify-end">
      <div
        class="overflow-x-auto whitespace-nowrap space-x-4 sm:space-x-16 py-3 rounded-lg"
      >
        {#each part23Forecast as topic, i}
          <a
            href="#section-{i}"
            on:click={(e) => {
              e.preventDefault(); // Prevent default anchor behavior
              handleClick(i);
            }}
            class="flex-shrink-0 text-md py-2 px-1 rounded-md sm:px-4 cursor-pointer hover:text-primary transition-colors duration-300"
            class:border-b-2={activeSection === i}
            class:border-black={activeSection === i}
            class:text-primary={activeSection === i}
          >
            {topic.topic}
          </a>
        {/each}
      </div>
    </div>
      
    <div
      class="flex overflow-x-auto whitespace-nowrap border-b-2 border-gray-200 text-sm sm:text-lg font-semibold mt-5 sm:mt-8 pb-3"
    >
      <p class="w-1/2 text-center">Part 2</p>
      <p class="w-1/2 text-center">Part 3</p>
    </div>
  </div>

  {#each part23Forecast as topic, i}
    {#if i > 0}
      <div class="divider"></div>
    {/if}

    <!-- Adjusted section with proper height for visibility -->
    <div id="section-{i}" class="h-5"></div>
    <h1 class="flex space-x-2 text-lg sm:text-2xl mt-5 font-semibold">
      <img src="/images/topic_part23.png" alt="Topic" class="w-7 sm:w-9 h-7 sm:h-9">
      <span>{topic.topic}</span>
    </h1>

    {#each topic.titles as item, j}
      {#if j > 0}
        <div class="divider"></div>
      {/if}

      <!-- Adjusted section id to match the topic and title indices -->
      <div id="section-{i}-{j}" class="pt-5"> 
        <div class="flex sm:items-center space-x-3 sm:space-x-10">
          <div class="w-1/2 mt-5">
            <div class="flex sm:items-center space-x-2 sm:space-x-7 bg-primary-content text-sm sm:text-lg text-center px-2 sm:px-5 py-4 rounded-lg">
              <img src="/images/describe.png" alt="Describe" class="w-0 sm:w-16 h-0 sm:h-16" />
              <span class="w-4/5">{item.title}</span>
            </div>

            <div class="mt-5">
              <p class="font-semibold text-sm sm:text-base mb-2"><em>Cue Card:</em></p>
              {#each item.cueCards as cueCart}
                <li class="ml-2 sm:ml-5 text-sm sm:text-base">{cueCart}</li>
              {/each}
            </div>
          </div>

          <div class="w-1/2 mt-5">
            {#each item.questions as question}
              <p class="flex space-x-2 sm:space-x-5 border border-primary-content px-2 sm:px-4 py-2 rounded-lg mb-5">
                <img src="/images/question.png" alt="Question" class="w-4 sm:w-7 h-4 sm:h-7" />
                <span class="text-sm sm:text-base">{question}</span>
              </p>
            {/each}
          </div>
        </div>
      </div>
    {/each}
  {/each}
</div>
