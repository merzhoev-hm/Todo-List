<script setup>
import { ref } from 'vue'

const newTodo = ref('')
const todosData = JSON.parse(localStorage.getItem('todos')) || []
const todos = ref(todosData)

const addTodo = () => {
  if (newTodo.value) {
    todos.value.push({
      done: false,
      content: newTodo.value,
      id: Date.now(),
    })
  }
  newTodo.value = ''
  saveData()
}

const removeTodo = todoId => {
  todos.value = todos.value.filter(todo => todo.id !== todoId)
  saveData()
}

const doneTodo = todo => {
  todo.done = !todo.done
  saveData()
}

const saveData = () => {
  const storageData = JSON.stringify(todos.value)
  localStorage.setItem('todos', storageData)
}
</script>

<template>
  <h1>Todo App</h1>
  <form @submit.prevent="addTodo" action="">
    <label for="">New todo</label>
    <input type="text" v-model="newTodo" />
    <button type="submit">Add new todo</button>
  </form>
  <h2>Todo List</h2>

  <ul>
    <li v-for="todo in todos" :key="todo.id">
      <span :class="{ done: todo.done }">{{ todo.content }}</span>
      <div>
        <button :class="{ donebtn: todo.done }" @click="doneTodo(todo)">
          ✓
        </button>
        <button class="removebtn" @click="removeTodo(todo.id)">X</button>
      </div>
    </li>
  </ul>

  <h4 v-if="!todos.length">Empty list.</h4>
</template>

<style scoped></style>
