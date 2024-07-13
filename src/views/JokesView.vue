<template>
  <div class="px-4 lg:px-8 py-10 min-h-screen">
    <CustomDropdownVue :currentValue="currentCategory" @onSelectMenuItem="handleSelectCategory" />

    <JokePanelVue
      v-if="currentStatus === Status.SUCCESS && joke"
      :joke="joke"
      @onGetMoreJokes="getJoke"
    />

    <div
      class="h-[300px] w-full flex items-center justify-center"
      v-else-if="(currentStatus === Status.SUCCESS && !joke) || currentStatus === Status.ERROR"
    >
      <p class="text-base text-gray-400 text-center max-w-[250px] mx-auto">
        {{
          currentStatus === Status.ERROR
            ? "Looks like something went wrong"
            : "No joke found for this category, select a different category"
        }}
      </p>
    </div>

    <LoadingSpinnerVue v-else-if="currentStatus === Status.LOADING" />
  </div>
</template>

<script setup lang="ts">
import CustomDropdownVue from "@/components/CustomDropdown.vue";
import JokePanelVue from "@/components/JokePanel.vue";
import axios from "axios";
import { ref, watch } from "vue";
import { JokeCategories, Status } from "@/enums";
import LoadingSpinnerVue from "@/components/LoadingSpinner.vue";

const currentStatus = ref<Status>(Status.IDLE);
const joke = ref<string>("");
const currentCategory = ref<JokeCategories>(JokeCategories.ANY);

function handleSelectCategory(category: JokeCategories) {
  currentCategory.value = category;
}

async function getJoke() {
  currentStatus.value = Status.LOADING;
  try {
    const response = await axios.get(
      `https://v2.jokeapi.dev/joke/${currentCategory.value.toLowerCase()}?blacklistFlags=nsfw,sexist,racist,political,religious,explicit`
    );
    const { status, data } = response || {};
    if (status === 200) {
      joke.value = data.type === "twopart" ? data.setup + " " + data.delivery : data.joke;
      currentStatus.value = Status.SUCCESS;
    }
  } catch (err) {
    currentStatus.value = Status.ERROR;
  }
}

watch(currentCategory, () => getJoke(), { immediate: true });
</script>
