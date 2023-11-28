<template>
	<main class="app">
	  <section class="greeting">
		<h2 class="title">
		  What's up, <input type="text" id="name" placeholder="Name here" v-model="name">
		</h2>
	  </section>
  
	  <section class="create-todo">
		<h3>Create a To-do</h3>
  
		<form id="new-todo-form" @submit.prevent="addTodo">
		  <h4>What's on your todo list?</h4>
		  <input 
			type="text" 
			name="content" 
			id="content" 
			placeholder="e.g. make a video"
			v-model="input_content" />
		  
		  <h4>Pick a category</h4>
		  <div class="options">
			<label>
			  <input 
				type="radio" 
				name="category" 
				id="category1" 
				value="business"
				v-model="input_category" />
			  <span class="bubble business"></span>
			  <div>Business</div>
			</label>
			<label>
			  <input 
				type="radio" 
				name="category" 
				id="category2" 
				value="personal"
				v-model="input_category" />
			  <span class="bubble personal"></span>
			  <div>Personal</div>
			</label>
		  </div>
  
		  <input type="submit" value="Add todo" />
		</form>
	  </section>
  
	  <section class="todo-list">
		<h3>TODO LIST</h3>
		<div class="tasks-container">
		<!-- Incomplete Tasks -->
			<div v-if="incompleteTodos.length > 0">
		  		<h4>Incomplete: {{ totalIncompleteTasks }} tasks</h4>
				<div class="list">
					<div v-for="todo in incompleteTodos" :key="todo.id" :class="`todo-item ${!todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
						<span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"></span>
					</label>
					<div class="todo-content1">
						<input type="text" v-model="todo.content" />
					</div>
					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Delete</button>
					</div>
					</div>
				</div>
			</div>
		<!-- Completed Tasks -->
			<div v-if="completedTodos.length > 0">
			<h4>Completed: {{ totalCompletedTasks }} tasks</h4>
			<div class="list">
				<div v-for="todo in completedTodos" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
				<label>
					<input type="checkbox" v-model="todo.done" />
					<span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"></span>
				</label>
				<div class="todo-content">
					<input type="text" v-model="todo.content" />
				</div>
				<div class="actions">
					<button class="delete" @click="removeTodo(todo)">Delete</button>
				</div>
				</div>
			</div>
			</div>
		</div>
	  </section>
	</main>
  </template>
  
  <script setup>
	import { ref, onMounted, computed, watch } from 'vue'
	
	const todos = ref([])
	const name = ref('')
	
	const input_content = ref('')
	const input_category = ref(null)
	
	const todos_asc = computed(() => todos.value.sort((a, b) => a.createdAt - b.createdAt))
	
	watch(name, (newVal) => {
		localStorage.setItem('name', newVal)
	})
	
	watch(todos, (newVal) => {
		localStorage.setItem('todos', JSON.stringify(newVal))
		
	}, {
		deep: true
	})

	watch(todos, (newVal) => {
	console.log('Task list modified:', newVal)
	}, { deep: true })

	
	const addTodo = () => {
		if (input_content.value.trim() === '' || input_category.value === null) {
		return
		}
	
		todos.value.push({
		id: todos.value.length + 1,
		content: input_content.value,
		category: input_category.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
		})
	}
	
	const removeTodo = (todo) => {
		todos.value = todos.value.filter((t) => t !== todo)
	}
	
	onMounted(() => {
		name.value = localStorage.getItem('name') || ''
		todos.value = JSON.parse(localStorage.getItem('todos')) || []
	})
	
	const completedTodos = computed(() => todos.value.filter(todo => todo.done))
	const incompleteTodos = computed(() => todos.value.filter(todo => !todo.done))

	const totalCompletedTasks = computed(() => completedTodos.value.length)
	const totalIncompleteTasks = computed(() => incompleteTodos.value.length)

	onMounted(() => {
	fetchInitialData()
	})

	const fetchInitialData = () => {
	const initialData = JSON.parse(localStorage.getItem('todos')) || []


	todos.value = initialData
	}
  </script>
  