<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
  cellSize: number
}>()

enum State {
  Open = 'open',
  Closed = 'closed',
  Flagged = 'flagged',
  Question = 'question',
}

const state = ref(State.Closed)

const css = {
  [State.Open]: 'bg-gray-400',
  [State.Closed]: 'bg-gray-300',
  [State.Flagged]: 'bg-red-500',
  [State.Question]: 'bg-yellow-500',
}

function toggleState() {
  switch (state.value) {
    case State.Closed:
      state.value = State.Flagged
      break
    case State.Flagged:
      state.value = State.Question
      break
    case State.Question:
      state.value = State.Closed
      break
  }
}
</script>

<template>
  <button
    class="flex items-center justify-center bg-gray border border-gray-300" :class="[css]" :style="{
      width: `${props.cellSize}px`,
      height: `${props.cellSize}px`,
      fontSize: `${0.75 * props.cellSize}px`,
    }" @contextmenu.prevent="toggleState()"
  >
    <span v-if="state === State.Flagged" class="ico-mdi-flag text-red-700" />
    <span v-else-if="state === State.Question" class="ico-mdi-help" />
  </button>
</template>

<style>
</style>
