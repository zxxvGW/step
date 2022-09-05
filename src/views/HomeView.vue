<script setup lang="ts">
	import { ref } from "vue"

	const act_name = ref<string>("")
</script>

<template>
	<div class="about">
		<h1>This is an home page</h1>
		<input type="text" v-model="act_name" placeholder="活动名称" />
	</div>
</template>
