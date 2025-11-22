<script setup>
import { ref } from 'vue'

let id = 0

const newTodo = ref('')

const todos = ref([])
const addTodo = () => {
	if (newTodo.value.trim() === '') return
	todos.value.push({
		id: id++,
		text: newTodo.value.trim(),
	})
	newTodo.value = ''
}
const removeTodo = todo => {
	todos.value = todos.value.filter(t => t != todo)
}
</script>

<template>
	<div class="container">
		<h1 class="title">TODO APP</h1>
		<p class="subtitle">Количество задач:</p>
		<div class="body">
			<form @submit.prevent="addTodo">
				<input v-model="newTodo" required />
				<button class="btn">Добавить</button>
			</form>
			<ul class="item-list">
				<li class="item" v-for="todo in todos" :key="todo.id">
					{{ todo.text }}
					<button class="btn__delete" @click="removeTodo(todo)">X</button>
				</li>
			</ul>
		</div>
	</div>
</template>

<style scoped>
.container {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}
.subtitle {
	font-size: 20px;
	margin-bottom: 10px;
}
.item {
	font-size: 24px;
}
.btn__delete {
	width: 20px;
	height: 30px;
}
</style>
