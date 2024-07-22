<template xmlns="http://www.w3.org/1999/html">
  <div>
    <h1>hi {{ msg }}</h1>
    <div @click="add">{{ count }}</div>
    <input type="text" v-model="val" @keydown.enter="addToDo" />
    <div>
      <button @click="addToDo">add</button>
      <button @click="clearToDo" v-if="doneCount>0">clear</button>
    </div>
    <ul>
      <li v-for="todo in todos">
        <input type="checkbox" v-model="todo.done">
        <span>
          {{ todo.title }}
        </span>
      </li>
    </ul>
    <div>
      {{ doneCount }}
      /
      {{ todos.length }}
    </div>
    <div>
      <input type="checkbox" v-model="allDone" />selectAll
    </div>
  </div>
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      msg: 'vuejs',
      count: 1,
      val: '',
      todos: [
        { title: 'learn', done: false },
        { title: 'talk', done: true }
      ]
    }
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
      deep: true,
      immediate: true
    }
  },
  computed: {
    doneCount() {
      return this.todos.filter(todo => todo.done).length
    },
    allDone: {
      get() {
        return this.todos.filter(todo => todo.done).length === this.todos.length
      },
      set(value) {
        this.todos.forEach(v => v.done = value)
      }
    }
  },
  methods: {
    add() {
      this.count++
    },
    addToDo() {
      this.todos.push({
        title: this.val,
        done: false
      })
      this.val = ''
    },
    clearToDo() {
      this.todos = this.todos.filter(todo => !todo.done)
    }
  }
})
</script>

<style>

</style>