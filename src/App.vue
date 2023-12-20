<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue';
import { Flower } from './components/HelloWorld.vue';

const game: Flower[][] = [
  [
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
    { color: 'sky', selected: false },
    { color: 'sky', selected: false },
    { color: 'purple', selected: false },
    { color: 'slate', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
  ],
  [
    { color: 'purple', selected: false },
    { color: 'purple', selected: false },
    { color: 'pink', selected: false },
    { color: 'slate', selected: false },
    { color: 'sky', selected: false },
    { color: 'purple', selected: false },
    { color: 'purple', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
  ],
  [
    { color: 'purple', selected: false },
    { color: 'purple', selected: false },
    { color: 'pink', selected: false },
    { color: 'orange', selected: false },
    { color: 'sky', selected: false },
    { color: 'purple', selected: false },
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
  ],
  [
    { color: 'slate', selected: false },
    { color: 'sky', selected: false },
    { color: 'pink', selected: false },
    { color: 'orange', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
    { color: 'pink', selected: false },
    { color: 'pink', selected: false },
    { color: 'orange', selected: false },
  ],
  [
    { color: 'pink', selected: false },
    { color: 'sky', selected: false },
    { color: 'sky', selected: false },
    { color: 'purple', selected: false },
    { color: 'purple', selected: false },
    { color: 'purple', selected: false },
    { color: 'pink', selected: false },
    { color: 'pink', selected: false },
    { color: 'slate', selected: false },
  ],
  [
    { color: 'pink', selected: false },
    { color: 'sky', selected: false },
    { color: 'yellow', selected: false },
    { color: 'pink', selected: false },
    { color: 'slate', selected: false },
    { color: 'sky', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
  ],
  [
    { color: 'purple', selected: false },
    { color: 'yellow', selected: false },
    { color: 'yellow', selected: false },
    { color: 'pink', selected: false },
    { color: 'sky', selected: false },
    { color: 'sky', selected: false },
    { color: 'orange', selected: false },
    { color: 'slate', selected: false },
    { color: 'sky', selected: false },
  ],
  [
    { color: 'purple', selected: false },
    { color: 'slate', selected: false },
    { color: 'yellow', selected: false },
    { color: 'pink', selected: false },
    { color: 'pink', selected: false },
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
    { color: 'orange', selected: false },
    { color: 'sky', selected: false },
  ],
];
</script>

<template>
  <div class="h-full w-full bg-green-100">
    <HelloWorld :game="game" />
  </div>
</template>
