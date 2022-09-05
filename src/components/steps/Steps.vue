<script setup lang="ts">
	import { ref, provide, watch, watchEffect } from "vue"
	const steps = ref([])
	const props = defineProps({
		active: {
			type: Number,
			default: 0,
		},
		finishStatus: {
			type: String,
			values: ["wait", "process", "finish", "error", "success"],
			default: "finish",
		},
		processStatus: {
			type: String,
			values: ["wait", "process", "finish", "error", "success"],
			default: "process",
		},
	})
	provide("Steps", { props, steps })

	watchEffect(() => {
		steps.value.forEach((instance, index) => {
			// @ts-ignore
			instance.setIndex(index)
		})
	})
</script>

<template>
	<div class="steps">
		<slot />
	</div>
</template>

<style>
	.steps {
		display: flex;
	}
</style>
