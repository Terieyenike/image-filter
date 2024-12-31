<template>
  <div class="my-8 px-4">
    <div class="flex justify-center">
      <div class="w-full max-w-lg">
        <div class="relative overflow-hidden">
          <canvas class="max-w-full h-auto block mx-auto" width="448" height="448" ref="canvasEl"></canvas>
        </div>
        <div class="text-white text-xl mt-4">
          <div class="flex justify-center gap-4">
            <select
              v-model="store.filter"
              class="py-4 w-full max-w-lg mx-auto bg-indigo-600 text-white text-center text-lg md:text-base"
            >
              <option value="">Select a filter</option>
              <option
                v-for="(filter, index) in filters"
                :key="index"
                :value="filter"
              >
                {{ filter }}
              </option>
            </select>
          </div>
          <a
            class="bg-indigo-700 py-4 block w-full mt-2 text-center text-lg md:text-base px-4"
            :href="canvasImgURL"
            download="image.png"
          >
            Download
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useImageStore } from "@/stores/image";
import useReader from "@/composables/use-reader";
import useCanvas from "@/composables/use-canvas";

const filters = [
  "oceanic", "vintage", "rosetint", "islands", "marine", "seagreen",
  "flagblue", "liquid", "diamante", "perfume", "serenity", "lofi",
  "cali", "obsidian", "firenze", "dramatic", "golden"
];
const store = useImageStore();
const { canvasEl, loadImage, drawOriginalImage, filterImage, canvasImgURL } =
  useCanvas();
const { reader } = useReader(store.file, () => {
  if (!reader.result) return;

  const dataURL = reader.result.toString();
  loadImage(dataURL);
});

store.$subscribe((mutation, state) => {
  drawOriginalImage();
  filterImage(state.filter);
});
</script>
