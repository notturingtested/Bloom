<script setup lang="ts">
import { ref } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import { games, Game } from "./games";
const game = ref<Game | null>(null);
function chooseGame(index) {
  game.value = games[index];
}
</script>

<template>
  <div class="h-screen w-screen bg-green-100">
    <div v-if="!game">
      <h1
        class="text-center text-4xl bold uppercase leading-loose text-green-700"
      >
        Bloom
      </h1>
      <h2 class="text-center text-2xl bold uppercase text-green-700">Games</h2>
      <div class="flex gap-1 justify-center mt-4">
        <div v-for="(game, index) in games" :key="game.Name" class="flex">
          <button
            @click="chooseGame(index)"
            class="bg-white shadow-sm py-2 px-4 text-green-700 rounded border border-green-700 hover:shadow-xl hover:border-green-900 hover:text-green-900 duration-500"
          >
            {{ game.Name }}
          </button>
        </div>
      </div>
    </div>
    <div v-else>
      <HelloWorld :game="games[0].Board" />
      <button
        @click="game = null"
        class="bg-white shadow-sm py-2 px-4 text-green-700 rounded border border-green-700 hover:shadow-xl hover:border-green-900 hover:text-green-900 duration-500 absolute right-0 bottom-0 m-1"
      >
        Back
      </button>
    </div>
  </div>
</template>
