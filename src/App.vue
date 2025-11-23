<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const todos = ref([])
const hideCompleted = ref(false)

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
const filteredTodos = computed(() => {
	return hideCompleted.value ? todos.value.filter(t => !t.done) : todos.value
})
</script>

<template>
	<div class="container">
		<h1 class="title">TODO APP</h1>
		<p class="subtitle">Количество задач: {{}}</p>
		<div class="body">
			<form @submit.prevent="addTodo">
				<input v-model="newTodo" required />
				<button class="btn">Добавить</button>
			</form>
			<ul class="item-list">
				<li class="item" v-for="todo in filteredTodos" :key="todo.id">
					<input type="checkbox" class="input__checkbox" v-model="todo.done" />
					<span :class="{ done: todo.done }">{{ todo.text }}</span>
					<button class="btn__delete" @click="removeTodo(todo)">X</button>
				</li>
			</ul>
			<button @click="hideCompleted = !hideCompleted">
				{{ hideCompleted ? 'Показать все' : 'Скрыть выполненные' }}
			</button>
		</div>
	</div>
</template>

<style scoped>
.done {
	text-decoration: line-through;
}

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

.input__checkbox {
	width: 20px;
	height: 20px;
}
</style>
