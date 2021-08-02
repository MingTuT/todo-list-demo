<template>
	<div class="todo-main">
		<ul class="todo-list">
			<ListItem v-for="(item,index) in todos" :key="item.id" :todoitem="item" :index="index" />
		</ul>
	</div>
</template>

<script lang="ts">
	import {
		defineComponent
	} from 'vue'
	import ListItem from "@/components/ListItem.vue";

	export default defineComponent({
		name: 'List',
		props: ['todos'],
		components: { ListItem },
		setup() {
			return {}
		}
	})
</script>

<style scoped>
	.todo-list {
		width: 100%;
	}
</style>
