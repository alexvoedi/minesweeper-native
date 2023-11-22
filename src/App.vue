<script setup lang="ts">
import { useLocalStorage } from '@vueuse/core'

const state = useLocalStorage('state', { count: 0 })
</script>

<template>
  <div>
    <button @click="state.count += 1">
      inc
    </button>
    {{ state }}
  </div>
</template>

<style>
</style>
