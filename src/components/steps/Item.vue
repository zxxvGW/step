<template>
	<div class="step">
		<!-- icon | index -->
		<div class="icon">
			<slot name="icon">
				<div v-if="currentStatus == 'finish'">
					<IconCommunity></IconCommunity>
				</div>
				<div :class="['icon-inner']" v-else>
					{{ index + 1 }}
				</div>
			</slot>
		</div>
		<!--  title & line  -->
		<div class="main">
			<slot name="title">{{ title }} - {{ currentStatus }}</slot>
			<div class="line"></div>
		</div>
	</div>
</template>
<script setup>
	import IconCommunity from "../icons/IconCommunity.vue"
	import { getCurrentInstance, inject, ref, computed, reactive, watch, onMounted } from "vue"
	const parent = inject("Steps")

	const index = ref(-1)
	const internalStatus = ref("")

	const props = defineProps({
		title: {
			type: String,
			default: () => "Step",
		},
		status: {
			type: String,
			values: ["", "wait", "process", "finish"],
			default: "",
		},
	})
	// 前一个step
	const prevStatus = computed(() => {
		const prevStep = parent.steps.value[index.value - 1]
		return prevStep ? prevStep.currentStatus : "wait"
	})
	//
	const isLast = computed(() => {
		return parent.steps.value[stepsCount.value - 1]?.uid === currentInstance?.uid
	})

	// 当前状态
	const currentStatus = computed(() => {
		return props.status || internalStatus.value
	})
	// 全部步骤条长度
	const stepsCount = computed(() => {
		return parent.steps.value.length
	})
	// 对外暴露，确定当前是第几个
	const setIndex = (val) => {
		index.value = val
	}
	// 组件实例
	const currentInstance = getCurrentInstance()
	// 更新状态
	const updateStatus = (activeIndex) => {
		if (activeIndex > index.value) {
			internalStatus.value = parent.props.finishStatus
		} else if (activeIndex === index.value && prevStatus.value !== "error") {
			internalStatus.value = parent.props.processStatus
		} else {
			internalStatus.value = "wait"
		}
		const prevChild = parent.steps.value[stepsCount.value - 1]
	}
	//
	const stepItemState = reactive({
		uid: computed(() => currentInstance?.uid),
		currentStatus,
		setIndex,
	})

	onMounted(() => {
		watch(
			() => parent.props.active,
			(active) => {
				updateStatus(active)
			},
			{ immediate: true }
		)
	})

	parent.steps.value = [...parent.steps.value, stepItemState]
</script>

<style>
	.step {
		display: flex;
	}
	.icon-inner {
		width: 28px;
		height: 28px;
		background: #f2f3f5;
		text-align: center;
		border-radius: 50%;
		color: #86909c;
	}
</style>
