<script lang="ts">
  import part1Forecast from "$lib/data/part1_forecast.json";
  import { onMount } from "svelte";

  let activeSection = 0;
  let isClicking = false; // To temporarily disable scroll logic

  onMount(() => {
    const handleScroll = () => {
      if (isClicking) return; // Ignore scroll events while clicking

      const sections = document.querySelectorAll("[id^='section-']");
      sections.forEach((section, index) => {
        const rect = section.getBoundingClientRect();
        if (rect.top <= 300 && rect.bottom >= 300) {
          activeSection = index;
        }
      });
    };

    window.addEventListener("scroll", handleScroll);
    return () => {
      window.removeEventListener("scroll", handleScroll);
    };
  });

  const handleClick = (index: number) => {
    isClicking = true; // Temporarily disable scroll logic
    activeSection = index;

    const section = document.querySelector(`#section-${index}`);
    section?.scrollIntoView({ behavior: "smooth" });

    // Re-enable scroll handling after scrolling finishes
    setTimeout(() => {
      isClicking = false;
    }, 500); // Adjust timeout based on scrolling duration
  };
</script>

<div class="my-10">
  <h1 class="text-center font-semibold text-4xl mb-10">
    Bộ đề dự đoán IELTS SPEAKING Part 1!
  </h1>

  <div
    class="flex overflow-x-auto whitespace-nowrap space-x-6 border-b-2 border-gray-200 mx-20 sticky top-0 bg-white"
  >
    {#each part1Forecast as item, i}
      <a
        href="#section-{i}"
        on:click={(e) => {
          e.preventDefault(); // Prevent default anchor behavior
          handleClick(i);
        }}
        class="flex-shrink-0 text-md py-3 px-4 cursor-pointer hover:text-blue-600 transition-colors duration-300"
        class:border-b-2={activeSection === i}
        class:border-black={activeSection === i}
        class:text-blue-600={activeSection === i}
      >
        {item.title}
      </a>
    {/each}
  </div>

  <div class="px-20 flex justify-center">
    <div>
      {#each part1Forecast as item, i}
        <div id="section-{i}" class="h-5"></div>
        <div 
          class="text-xl flex font-medium mt-10 mb-5 border border-base-300 rounded-lg p-4 shadow-sm hover:shadow-md 
          transition-shadow duration-300 bg-primary-content"
        >
          <img src="src/images/title.png" alt="Title" class="w-9 h-9 mr-4">
          <span class="mt-1">{item.title}</span>
        </div>

        <div class="mb-10 space-y-5 ml-10">
          {#each item.questions as question}
            <div class="flex space-x-2">
              <img src="src/images/question.png" alt="Question" class="w-7 h-7">
              <span>{question}</span>
            </div>
          {/each}
        </div>  
      {/each}
    </div>
  </div>
</div>
