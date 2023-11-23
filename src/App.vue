<script setup lang="ts">
import { useLocalStorage } from '@vueuse/core'
import { computed, ref } from 'vue';
import Cell from '@/components/Cell.vue'

enum Difficulty {
	Beginner = 'beginner',
	Intermediate = 'intermediate',
	Expert = 'expert'
}

const difficulties = {
	[Difficulty.Beginner]: {
		rows: 9,
		columns: 9,
		mines: 10
	},
	[Difficulty.Intermediate]: {
		rows: 16,
		columns: 16,
		mines: 40
	},
	[Difficulty.Expert]: {
		rows: 16,
		columns: 30,
		mines: 99
	}
}

const storage = useLocalStorage('minesweeper', {
	highscores: {
		beginner: [],
		intermediate: [],
		expert: [],
	},
	settings: {
		difficulty: Difficulty.Beginner,
		theme: 'light',
		cellSize: 24,
	}
})

const timer = ref(0)

const settings = computed(() => difficulties[storage.value.settings.difficulty])

const openCell = (x: number, y: number) => {
	console.log('openCell', x, y)
}
</script>

<template>
	<div>
		<div>
			<h1>Minesweeper</h1>
		</div>

		<button @click="storage.settings.cellSize += 1">increase</button>
		<button @click="storage.settings.cellSize -= 1">decrease</button>

		<div :style="{
			display: 'grid',
			gridTemplateColumns: `repeat(${settings.columns}, ${storage.settings.cellSize}px)`,
			gridTemplateRows: `repeat(${settings.rows}, ${storage.settings.cellSize}px)`,
		}">
			<div v-for="(row, rowIndex) of settings.rows" :key="rowIndex">
				<div v-for="(column, columnIndex) of settings.columns" :key="columnIndex">
					<Cell @click="() => openCell(columnIndex, rowIndex)" :cellSize="storage.settings.cellSize" />
				</div>
			</div>
		</div>
	</div>
</template>

<style>
</style>
