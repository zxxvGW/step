<script setup lang="ts">
	import { ref, unref } from "vue"
	const msg = ref("")
	// 原始值
	const price = "99"
	// 改变值
	const mark_price = ref("99")
	// 是否已经输入过
	const changed = ref(false)
	// 清除
	const clear = () => {
		changed.value = false
	}
	// 聚焦
	const show = () => {
		changed.value = true
		getMsg()
	}
	// 每次改变检查
	const change = () => {
		console.log("cahnge")
		if (!changed.value) return
		changed.value = true
	}

	const getMsg = () => (msg.value = unref(mark_price) == price ? "相同" : "")
</script>

<template>
	<div class="about">
		<h2>{{ price }}</h2>
		<input
			:class="msg && changed ? 'err' : ''"
			@focus="clear"
			v-model="mark_price"
			placeholder="活动名称"
			@blur="show"
			@input="change"
		/>
		<span class="error" v-show="changed">{{ msg }}</span>
	</div>
</template>

<style scoped>
	.error {
		color: red;
	}
	.err {
		border-color: red;
	}
</style>
