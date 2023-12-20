<script setup lang="ts">
import x from './x.vue';

import { ref, computed, onMounted } from 'vue';
interface Flower {
  color: string;
  selected: boolean;
}
const options: Flower[] = [
  { color: 'pink', selected: false },
  { color: 'orange', selected: false },
  { color: 'purple', selected: false },
  { color: 'yellow', selected: false },
  { color: 'green', selected: false },
  { color: 'slate', selected: false },
];
const xx = options.map((option: Flower) => {
  return { ...option, score: [false, false, false] };
});
const colorScore = ref(xx);
const game = ref<Flower[][]>([]);

//randomize the array
for (let i = 0; i < 10; ++i) {
  const currentRow = [];
  for (let j = 0; j < 9; j++) {
    const randomIndex = Math.floor(Math.random() * options.length);
    currentRow.push({ ...options[randomIndex] });
  }
  game.value.push(currentRow);
}

function flowerClasses(flower: Flower) {
  if (!flower.selected) {
    return `text-${flower.color}-500 hover:text-${flower.color}-700 border-${flower.color}-500 duration-500`;
  } else {
    return `text-${flower.color}-400 hover:text-${flower.color}-600 border-${flower.color}-500 duration-500`;
  }
}

const chunks = [];
for (let i = 0; i < game.value.length; i += 5) {
  let chunk = game.value.slice(i, i + 5);
  for (let j = 0; j < chunk[0].length; j += 3) {
    let subChunk = chunk.map((row) => row.slice(j, j + 3));
    chunks.push(subChunk);
  }
}
function select(bedIndex: number, rowIndex: number, flowerIndex: number) {
  const flower = flowerBeds.value[bedIndex][rowIndex][flowerIndex];
  if (flower.selected) {
    flower.selected = false;
  } else {
    flower.selected = true;
  }
  flowerBeds.value[bedIndex][rowIndex][flowerIndex] = flower;
}
const flowerBeds = ref(chunks);
const rolling = ref(false);
const unhappy = ref(false);
const unhappiness = ref(0);
const bedScore = ref([false, false, false, false]);
const negativePoints = ref(-1);
const Points = computed(() => {
  let beds = 0;
  for (let i = 0; i < bedScore.value.length; i++) {
    if (bedScore.value[i]) {
      beds = beds + i + 3;
    }
  }
  for (let i = 0; i < colorScore.value.length; i++) {
    const color = colorScore.value[i];
    if (color.color === 'slate') {
      continue;
    }
    let score = 0;
    for (let j = 0; j < color.score.length; j++) {
      if (color.score[j]) {
        //0 should be 6, 1 should be 4, 2 should be 2
        score = score + (3 - j) * 2;
      }
    }

    beds = beds + score;
  }
  return (negativePoints.value + unhappiness.value) * -1 + beds;
});
async function rollDice() {
  rolling.value = true;
  negativePoints.value = negativePoints.value + 1;
  await new Promise((r) => setTimeout(r, 2000));
  rolling.value = false;
}
async function sad() {
  unhappy.value = true;
  unhappiness.value = unhappiness.value + 1;
  await new Promise((r) => setTimeout(r, 2000));
  unhappy.value = false;
}
function toggleScore(colorIndex: number, scoreIndex: number) {
  colorScore.value[colorIndex].score[scoreIndex] =
    !colorScore.value[colorIndex].score[scoreIndex];
  colorScore.value.splice(colorIndex, 1, colorScore.value[colorIndex]);
}
const elem = ref<HTMLElement | null>(null);

onMounted(() => {
  elem.value = document.documentElement; // or a specific element
});

// const toggleFullscreen = () => {
//   if (!elem.value) return;

//   if (!document.fullscreenElement) {
//     if (elem.value.requestFullscreen) {
//       elem.value.requestFullscreen();
//     } else if ((elem.value as any).mozRequestFullScreen) {
//       /* Firefox */
//       (elem.value as any).mozRequestFullScreen();
//     } else if ((elem.value as any).webkitRequestFullscreen) {
//       /* Chrome, Safari, Opera */
//       (elem.value as any).webkitRequestFullscreen();
//     } else if ((elem.value as any).msRequestFullscreen) {
//       /* IE/Edge */
//       (elem.value as any).msRequestFullscreen();
//     }
//   } else {
//     if (document.exitFullscreen) {
//       document.exitFullscreen();
//     } else if ((document as any).mozCancelFullScreen) {
//       /* Firefox */
//       (document as any).mozCancelFullScreen();
//     } else if ((document as any).webkitExitFullscreen) {
//       /* Chrome, Safari, Opera */
//       (document as any).webkitExitFullscreen();
//     } else if ((document as any).msExitFullscreen) {
//       /* IE/Edge */
//       (document as any).msExitFullscreen();
//     }
//   }
// };
</script>

<template>
  <div class="flex justify-around md:p-16 h-screen">
    <div class="w-5/6 h-full mr-1">
      <div class="grid grid-cols-3 gap-1 h-full">
        <div
          v-for="(bed, bedIndex) in flowerBeds"
          :key="bedIndex"
          class="rounded-xl bg-white h-full w-full"
        >
          <div
            v-for="(row, rowIndex) in bed"
            class="grid grid-cols-3 gap-y-1 h-1/5"
            :key="rowIndex"
          >
            <div
              v-for="(flower, flowerIndex) in row"
              class="cursor-pointer relative m-1 h-full flex place-content-center pt-2"
              :class="flowerClasses(flower)"
              @click="select(bedIndex, rowIndex, flowerIndex)"
            >
              <div
                class="absolute top-0 left-0 w-full h-full flex items-center justify-center z-40"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 512 512"
                  fill="currentColor"
                  class="inline-block h-full"
                  style="vertical-align: -0.125em"
                >
                  <path
                    d="M258.6 0c-1.7 0-3.4 .1-5.1 .5C168 17 115.6 102.3 130.5 189.3c2.9 17 8.4 32.9 15.9 47.4L32 224H29.4C13.2 224 0 237.2 0 253.4c0 1.7 .1 3.4 .5 5.1C17 344 102.3 396.4 189.3 381.5c17-2.9 32.9-8.4 47.4-15.9L224 480v2.6c0 16.2 13.2 29.4 29.4 29.4c1.7 0 3.4-.1 5.1-.5C344 495 396.4 409.7 381.5 322.7c-2.9-17-8.4-32.9-15.9-47.4L480 288h2.6c16.2 0 29.4-13.2 29.4-29.4c0-1.7-.1-3.4-.5-5.1C495 168 409.7 115.6 322.7 130.5c-17 2.9-32.9 8.4-47.4 15.9L288 32V29.4C288 13.2 274.8 0 258.6 0zM256 224a32 32 0 1 1 0 64 32 32 0 1 1 0-64z"
                  />
                </svg>
              </div>
              <div
                class="absolute top-0 left-0 w-full h-full flex items-center justify-center z-50 text-gray-900"
                v-if="flower.selected"
              >
                <x />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--  -->
    <!--  -->
    <!--  -->
    <div class="flex flex-col rounded-xl bg-green-300 w-1/4 h-full">
      <div v-for="(option, index) in colorScore" :key="index">
        <div
          v-if="option.color !== 'slate'"
          class="border-2 rounded-full flex items-center bg-white m-1 font-size-xl"
          :class="flowerClasses(option)"
        >
          <div class="ml-1">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 512 512"
              fill="currentColor"
              class="inline-block"
              style="height: 1em; vertical-align: -0.125em"
            >
              <path
                d="M258.6 0c-1.7 0-3.4 .1-5.1 .5C168 17 115.6 102.3 130.5 189.3c2.9 17 8.4 32.9 15.9 47.4L32 224H29.4C13.2 224 0 237.2 0 253.4c0 1.7 .1 3.4 .5 5.1C17 344 102.3 396.4 189.3 381.5c17-2.9 32.9-8.4 47.4-15.9L224 480v2.6c0 16.2 13.2 29.4 29.4 29.4c1.7 0 3.4-.1 5.1-.5C344 495 396.4 409.7 381.5 322.7c-2.9-17-8.4-32.9-15.9-47.4L480 288h2.6c16.2 0 29.4-13.2 29.4-29.4c0-1.7-.1-3.4-.5-5.1C495 168 409.7 115.6 322.7 130.5c-17 2.9-32.9 8.4-47.4 15.9L288 32V29.4C288 13.2 274.8 0 258.6 0zM256 224a32 32 0 1 1 0 64 32 32 0 1 1 0-64z"
              />
            </svg>
          </div>
          <div class="px-2 flex">
            <div class="relative cursor-pointer" @click="toggleScore(index, 0)">
              6
              <div
                class="absolute z-50 top-0 left-0 text-gray-700 font-bold"
                v-if="option.score[0]"
              >
                X
              </div>
            </div>
            -
            <div
              class="relative cursor-pointer"
              @click="option.score[1] = !option.score[1]"
            >
              4
              <div
                class="absolute z-50 top-0 left-0 text-gray-700 font-bold"
                v-if="option.score[1]"
              >
                X
              </div>
            </div>
            -
            <div
              class="relative cursor-pointer"
              @click="option.score[2] = !option.score[2]"
            >
              2
              <div
                class="absolute z-50 top-0 left-0 text-gray-700 font-bold"
                v-if="option.score[2]"
              >
                X
              </div>
              <!-- {{ option.score[2] ? 'X' : 2 }} -->
            </div>
          </div>
        </div>
      </div>
      <!--  -->
      <!--  -->
      <div class="text-green-700 text-center">
        <div class="uppercase">Garden Beds</div>
        <div class="flex justify-around">
          <div
            class="bg-white rounded-full p-1 relative cursor-pointer"
            @click="bedScore[0] = !bedScore[0]"
          >
            3
            <div
              class="absolute z-50 top-0 left-0 text-gray-700 font-bold p-1"
              v-if="bedScore[0]"
            >
              X
            </div>
          </div>
          <div
            class="bg-white rounded-full p-1 relative cursor-pointer"
            @click="bedScore[1] = !bedScore[1]"
          >
            4
            <div
              class="absolute z-50 top-0 left-0 text-gray-700 font-bold p-1"
              v-if="bedScore[1]"
            >
              X
            </div>
          </div>
          <div
            class="bg-white rounded-full p-1 relative cursor-pointer"
            @click="bedScore[2] = !bedScore[2]"
          >
            5

            <div
              class="absolute z-50 top-0 left-0 text-gray-700 font-bold p-1"
              v-if="bedScore[2]"
            >
              X
            </div>
          </div>
          <div
            class="bg-white rounded-full p-1 relative cursor-pointer"
            @click="bedScore[3] = !bedScore[3]"
          >
            6
            <div
              class="absolute z-50 top-0 left-0 text-gray-700 font-bold p-1"
              v-if="bedScore[3]"
            >
              X
            </div>
          </div>
        </div>
      </div>
      <!--  -->
      <!--  -->

      <div
        class="border-2 border-green-700 bg-white my-2 rounded-xl font-bold mx-1 flex flex-col"
      >
        <div class="grid grid-cols-2">
          <div
            class="flex justify-center items-center text-green-700 cursor-pointer"
            @click="sad"
            :class="{ 'animate-bounce': unhappy }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 512 512"
              class="inline-block"
              style="height: 1em; vertical-align: -0.125em"
            >
              <path
                d="M464 256A208 208 0 1 0 48 256a208 208 0 1 0 416 0zM0 256a256 256 0 1 1 512 0A256 256 0 1 1 0 256zM174.6 384.1c-4.5 12.5-18.2 18.9-30.7 14.4s-18.9-18.2-14.4-30.7C146.9 319.4 198.9 288 256 288s109.1 31.4 126.6 79.9c4.5 12.5-2 26.2-14.4 30.7s-26.2-2-30.7-14.4C328.2 358.5 297.2 336 256 336s-72.2 22.5-81.4 48.1zM144.4 208a32 32 0 1 1 64 0 32 32 0 1 1 -64 0zm192-32a32 32 0 1 1 0 64 32 32 0 1 1 0-64z"
              />
            </svg>
          </div>
          <div class="text-green-700 flex items-center justify-center">
            {{ unhappiness }}
          </div>
        </div>

        <div class="grid grid-cols-2">
          <div
            class="border-green-700 border-r-2 border-t-2 flex items-center justify-center"
          >
            <div
              class="text-green-700 flex justify-center items-center cursor-pointer"
              @click="rollDice"
              :class="{ 'animate-spin': rolling }"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="currentColor"
                class="inline-block"
                style="height: 1em; vertical-align: -0.125em"
                viewBox="0 0 448 512"
              >
                <path
                  d="M64 32C28.7 32 0 60.7 0 96V416c0 35.3 28.7 64 64 64H384c35.3 0 64-28.7 64-64V96c0-35.3-28.7-64-64-64H64zm64 96a32 32 0 1 1 0 64 32 32 0 1 1 0-64zM96 352a32 32 0 1 1 64 0 32 32 0 1 1 -64 0zM224 224a32 32 0 1 1 0 64 32 32 0 1 1 0-64zm64-64a32 32 0 1 1 64 0 32 32 0 1 1 -64 0zm32 160a32 32 0 1 1 0 64 32 32 0 1 1 0-64z"
                />
              </svg>
              <div class="pl-1">
                {{ negativePoints === -1 ? 1 : '' }}
              </div>
            </div>
          </div>
          <div
            class="text-green-700 border-t-2 border-dotted border-green-700 flex items-center justify-center"
          >
            {{ negativePoints === -1 ? '' : negativePoints * -1 }}
          </div>
        </div>
      </div>

      <div
        class="border-2 border-green-700 bg-white h-fit rounded-xl font-bold mx-1"
      >
        <div class="text-green-700 ml-1">Total: {{ Points }}</div>
      </div>
    </div>
  </div>
  <div class="invisible">
    <div class="text-pink-500 hover:text-pink-700 border-pink-500"></div>
    <div class="text-orange-500 hover:text-orange-700 border-orange-500"></div>
    <div class="text-purple-500 hover:text-purple-700 border-purple-500"></div>
    <div class="text-yellow-500 hover:text-yellow-700 border-yellow-500"></div>
    <div class="text-green-500 hover:text-green-700 border-green-500"></div>
    <div class="text-slate-500 hover:text-slate-700 border-slate-500"></div>
    <div class="text-pink-400 hover:text-pink-600"></div>
    <div class="text-orange-400 hover:text-orange-600"></div>
    <div class="text-purple-400 hover:text-purple-600"></div>
    <div class="text-yellow-400 hover:text-yellow-600"></div>
    <div class="text-green-400 hover:text-green-600"></div>
    <div class="text-slate-400 hover:text-slate-600"></div>
  </div>
  <!-- <div class="flex justify-center items-center absolute bottom-0 left-0">
    <button
      class="bg-green-700 p-4 rounded-lg hover:bg-green-800 duration-500"
      @click="toggleFullscreen"
    >
      <div class="text-white uppercase leading-loose bold">Go Fullscreen</div>
    </button>
  </div> -->
</template>
