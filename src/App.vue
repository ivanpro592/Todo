<script setup>
import { ref, computed } from 'vue'

let id = 0
let isDarkTheme = ref(false)

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
	todos.value = todos.value.filter(t => t.id != todo.id)
}
const filteredTodos = computed(() => {
	return hideCompleted.value ? todos.value.filter(t => !t.done) : todos.value
})

const amountTodos = computed(() => {
	return todos.value.length
})
</script>

<template>
	<div class="container" :class="{ 'is-dark': isDarkTheme }">
		<h1 class="title">TODO APP</h1>
		<p class="subtitle">Количество задач: {{ amountTodos }}</p>
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
			<button class="btn-show" @click="hideCompleted = !hideCompleted">
				{{ hideCompleted ? 'Показать все' : 'Скрыть выполненные' }}
			</button>
		</div>
		<button class="btn-change" @click="isDarkTheme = !isDarkTheme">
			Change theme
		</button>
	</div>
</template>

<style scoped>
.btn-change {
	margin-top: 25px;
}

.is-dark {
	background-color: black;
	color: white;
}
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
