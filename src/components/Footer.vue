<template>
	<div class="todo-footer">
		<div>
			<label><input type="checkbox" v-model="isCheckAll" /></label>
			<span class="todo-tag">已完成 {{ count }} /全部 {{ todos.length }}</span>
		</div>
		<button class="btn btn-danger" @click="clearCompleted">清除已完成任务</button>
	</div>
</template>

<script lang="ts">
	import {
		defineComponent,
		computed
	} from 'vue'
	import {
		Todo
	} from '../types/todo'

	export default defineComponent({
		name: "Footer",
		props: {
			todos: {
				type: Array as() => Todo[],
				required: true
			},
			checkAll: {
				type: Function,
				required: true
			},
			clearCompleted: {
				type: Function,
				required: true
			}
		},
		setup(props, context) {
			const count = computed(() => {
				return props.todos.reduce((pre: number, todo: Todo) => pre + (todo.isCompleted ? 1 : 0), 0)
			})
			const isCheckAll = computed({
				get() {
					return count.value > 0 && count.value === props.todos.length
				},
				set(val) {
					props.checkAll(val)
				}
			})
			return {
				count,
				isCheckAll
			}
		}
	})
</script>

<style scoped>
	.todo-footer {
		color: #fff;
		background-color: #e95b47;
		border: none;
		border-radius: 5px;
		padding: 5px;
		cursor: pointer;
	}

	.btn-danger {
		color: #fff;
		background-color: #e95b47;
		border: none;
		border-radius: 5px;
		padding: 5px;
		cursor: pointer;
	}

	.todo-tag {
		margin-left: 15px;
	}
</style>
