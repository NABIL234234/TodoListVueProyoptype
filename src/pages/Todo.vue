<script setup>
import Todo from './Todo.vue'
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([])

function addTodo() {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ text: newTodo.value, completed: false })
    newTodo.value = ''
  }
}

function removeTodo(index) {
  todos.value.splice(index, 1)
}

function toggleCompleted(index) {
  todos.value[index].completed = !todos.value[index].completed
}

// Вычисляемое свойство для сортировки выполненных задач внизу
const sortedTodos = computed(() => {
  return [...todos.value.filter(todo => !todo.completed), ...todos.value.filter(todo => todo.completed)]
})
</script>

<template>
  <div id="app">
    <h1>Список задач</h1>
    <div class="wrapper">
      <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new todo" />
      <img class="scaleAdd scale-hover" src="/add.svg" @click="addTodo"></img>
    </div>
    <transition-group name="fade">
      <ul>
        <li v-for="(todo, index) in todos" :key="index" class="fade">
          <img class="scale scale-hover" src="/done.svg" @click="toggleCompleted(index)" />
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
          <img class="scale scale-hover" src="/delete.svg" @click="removeTodo(index)" />
        </li>
      </ul>
    </transition-group>
  </div>
</template>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background: linear-gradient(90deg, #1cb5e0 0%, #000851 100%);
}
#app {
  max-width: 400px;
  margin: 20px auto;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(5, 0, 0, 0);
  padding: 20px;
}

.wrapper {
  display: flex;
  gap: 30px;
}

h1 {
  color: #ffffff;
  font-size: 24px;
  margin-bottom: 20px;
  text-align: center;
  font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}
input[type='text'] {
  width: 100%;
  padding: 8px;
  border: none;
  outline: none;
  border-radius: 20px;
  margin-bottom: 10px;
}
button {
  width: 90px;
  height: 30px;
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}
ul {
  list-style-type: none;
  padding: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
li {
  margin-bottom: 10px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
li button {
  background-color: #dc3545;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
li button:hover {
  background-color: #bd2130;
}
.completed {
  text-decoration: line-through;
  color: #888;
}

.scale-hover{
  transition: .7s;
}
.scale {
  width: 5%;
}

.scale:hover{
  transform: scale(1.5); /* Увеличиваем масштаб */
  transition: 0.5s;
}


.scaleAdd{
  width: 10%;
  padding-bottom: 12px;
}

.scaleAdd:hover{
  transform: scale(1.3); /* Увеличиваем масштаб */
  transition: 0.5s;
}

.fade{
  color:#000851;
}

@media (max-width:380px){
 .wrapper{
  display: block;
 }

 button{
  margin-left: 75px;
 }
}
</style>