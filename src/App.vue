<template>
	<div class="todo-wrap">
		<h2>todoList</h2>
		<Header :addToPlan="addToPlan" />
		<List :todos="todos" />
		<Footer :todos ='todos' :checkAll="checkAll" :clearCompleted="clearCompleted" />
	</div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, provide, watch, onMounted } from 'vue';
import { saveTodos, readTodos } from "@/utils/localStorageUtils";
import { Todo } from '@/types/todo'
import Header from './components/Head.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'

export default defineComponent({
  name: 'App',
  components: {
    Header,
		List,
		Footer
  },
	setup() {
		const state = reactive<{ todos: Todo[] }>({
			todos: []
		})
		onMounted(() => {
			setTimeout(() => { state.todos = readTodos() }, 1000)
		})
		watch(() => state.todos, saveTodos, { deep: true })
		// 用于head组件添加数据
		const addToPlan = (todo: Todo) => {
			state.todos.unshift(todo)
		}
		const updateState = (todo: Todo, val: boolean) => {
			todo.isCompleted = val
		}
		const delTodo: Function = (index: number) =>  {
			state.todos.splice(index, 1)
		}
		const checkAll = (val: boolean) => {
			state.todos.forEach(item => {
				item.isCompleted = val
			})
		}
		const clearCompleted = () => {
			state.todos = state.todos.filter((item, index) => {
				if(item.isCompleted) {
					return false
				}
				return true
			})
		}
		
		provide('updateState',updateState)
		provide('delTodo', delTodo)
		return {
			// 将state中的所有属性转换为ref对象 ，用的时候直接使用属性名称即可
			...toRefs(state),
			addToPlan,
			checkAll,
			clearCompleted
		}
	}
});
</script>

<style>
	.todo-wrap {
		width: 500px;
		margin: 0 auto;
		border: 1px solid lightblue;
		border-radius: 5px;
		padding: 20px;
	}
	.todo-wrap h2 {
		text-align: center;
	}
</style>
