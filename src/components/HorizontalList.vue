<template>
	<div class="horizontal-list" ref="horizontalList">
		<div
			v-for="(item, index) in items"
			:key="index"
			class="horizontal-item"
			:class="{ visible: isVisible(index) }"
			@mouseover="handleMouseOver(index)"
			@mouseleave="handleMouseLeave(index)"
		>
			{{ item.value }}
		</div>
	</div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, toRefs } from "vue"

const props = defineProps({
	items: {
		type: Array,
		required: true,
	},
})

const { items } = toRefs(props)
const horizontalList = ref(null)

const windowHeight = ref(window.innerHeight)

const updateRandomNumber = () => {
	const index = Math.floor(Math.random() * items.value.length)
	const newItem = { ...items.value[index], value: generateRandomNumber() }
	items.value.splice(index, 1, newItem)
}

const handleMouseOver = index => {
	items.value[index].isHovered = true
}

const handleMouseLeave = index => {
	items.value[index].isHovered = false
}

const generateRandomNumber = () => {
	return Math.floor(Math.random() * 100)
}

const isVisible = index => {
	if (!horizontalList.value) return false
	const el = horizontalList.value.children[index]
	if (!el) return false
	const rect = el.getBoundingClientRect()
	return rect.top < windowHeight.value && rect.bottom > 0
}

const updateVisibleItems = () => {
	windowHeight.value = window.innerHeight
}

onMounted(() => {
	updateVisibleItems()
	startUpdatingRandomNumber()
})

onUnmounted(() => {
	stopUpdatingRandomNumber()
})

let timer

const startUpdatingRandomNumber = () => {
	timer = setInterval(updateRandomNumber, 1000)
	window.addEventListener("scroll", updateVisibleItems)
	window.addEventListener("resize", updateVisibleItems)
}

const stopUpdatingRandomNumber = () => {
	clearInterval(timer)
	window.removeEventListener("scroll", updateVisibleItems)
	window.removeEventListener("resize", updateVisibleItems)
}
</script>

<style scoped>
.horizontal-list {
	display: flex;
	flex-wrap: wrap;
}

.horizontal-item {
	width: 1rem;
	border: 1px solid #000;
	border-radius: 5px;
	padding: 10px;
	margin-right: 10px;
	margin-bottom: 10px;
	transition: all 0.3s ease;
	opacity: 0;
}

.horizontal-item.visible {
	opacity: 1;
}

.horizontal-item:hover {
	transform: scale(0.8);
}
</style>
