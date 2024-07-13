<template>
  <div class="w-fit relative">
    <button
      class="flex h-fit p-2 border border-slate-200 text-sm font-medium capitalize w-[108px] items-center justify-center"
      @click="
        (e) => {
          e.stopPropagation();
          showMenu = !showMenu;
        }
      "
    >
      {{ currentValue.toLowerCase() }}
    </button>

    <ul
      class="w-[180px] absolute z-[10] p-2 left-0 bg-white shadow-md transition-all duration-[0.3s] ease-in-out border border-slate-200 flex flex-col gap-2"
      :class="{
        'opacity-100 pointer-events-auto visible top-[40px]': showMenu,
        'opacity-0 pointer-events-none invisible top-[38px]': !showMenu
      }"
      @click="(e) => e.stopPropagation()"
    >
      <li
        role="button"
        v-for="(category, index) in categories"
        :key="index"
        class="text-sm capitalize flex items-center gap-2"
        @click="
          () => {
            showMenu = false;
            $emit('onSelectMenuItem', category);
          }
        "
      >
        <input
          type="radio"
          :checked="currentValue === category"
          class="!cursor-pointer flex-shrink-0"
        />
        {{ category.toLowerCase() }}
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from "vue";
import { JokeCategories } from "../enums";

type Props = { currentValue: string };

type Emits = { onSelectMenuItem: [category: JokeCategories] };

const showMenu = ref<boolean>(false);

defineProps<Props>();

defineEmits<Emits>();

function onClickOutside() {
  showMenu.value = false;
}

const categories = computed(() => Object.values(JokeCategories));

onMounted(() => document.body.addEventListener("click", onClickOutside));

onUnmounted(() => document.body.removeEventListener("click", onClickOutside));
</script>

<style scoped></style>
