<template>
  <div class="min-h-[400px] flex flex-col items-center justify-center mt-4">
    <div class="w-[90%] max-w-[500px] relative">
      <div
        class="rounded-tl-md rounded-tr-md h-[18px] w-[95%] bg-white border border-b-0 border-b-[none] border-slate-200 absolute top-[-10px] left-[50%] translate-x-[-50%] z-[1]"
      ></div>
      <div
        class="w-full p-4 rounded-md border border-slate-200 relative z-[2] bg-white min-h-[200px] flex flex-col items-center justify-center gap-6"
      >
        <p class="text-sm text-center">
          {{ joke }}
        </p>

        <button
          class="flex items-center justify-center w-[150px] px-4 py-3 bg-slate-600 font-medium text-sm text-white"
          @click="$emit('onGetMoreJokes')"
        >
          Get more jokes
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
type Props = {
  joke: string;
};

type Emits = { onGetMoreJokes: [] };

defineProps<Props>();

defineEmits<Emits>();
</script>

<style scoped></style>
