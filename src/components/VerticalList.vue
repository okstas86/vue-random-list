<template>
	<div class="vertical-list">
		<div v-for="(item, index) in items" :key="index" class="vertical-item">
			<HorizontalList :items="item.horizontalItems" />
		</div>
	</div>
</template>

<script setup>
import { toRefs } from "vue"
import HorizontalList from "./HorizontalList.vue"

const props = defineProps({
	items: {
		type: Array,
		required: true,
	},
})

const { items } = toRefs(props)
</script>

<style scoped>
.vertical-list {
	display: flex;
	flex-direction: column;
}

.vertical-item {
	margin-bottom: 20px;
}
</style>
