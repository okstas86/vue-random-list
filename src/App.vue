<template>
	<div id="app">
		<VerticalList :items="verticalItems" />
	</div>
</template>

<script setup>
import VerticalList from "./components/VerticalList.vue"
import { ref } from "vue"

const verticalItems = ref([])
const generateVerticalList = () => {
	const count = Math.floor(Math.random() * 100) + 1
	for (let i = 0; i < count; i++) {
		const horizontalItems = []
		const horizontalItemCount = Math.floor(Math.random() * 90) + 10
		for (let j = 0; j < horizontalItemCount; j++) {
			horizontalItems.push({
				value: Math.floor(Math.random() * 100),
				isHovered: false,
			})
		}
		verticalItems.value.push({ horizontalItems })
	}
}

generateVerticalList()
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	margin-top: 60px;
}
</style>
