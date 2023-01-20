<script>
import { ref } from 'vue';
export default {
    setup() {
        const newTodo = ref("")
        const defaultTodo = [{
            done: false,
            content: "Master Vue"
        }]
        const data = JSON.parse(localStorage.getItem("todos")) || defaultTodo
        const todos = ref(data)
        function add() {
            if (newTodo.value) {
                todos.value.push({
                    done: false,
                    content: newTodo.value
                })
                newTodo.value = ""
            }
            localStorage.setItem("todos", JSON.stringify(todos.value))
        }
        function finish(todo) {
            if (!todo.done) {
                todo.done = !todo.done
            }
            localStorage.setItem("todos", JSON.stringify(todos.value))
        }
        function remove(index) {
            todos.value.splice(index, 1)
            localStorage.setItem("todos", JSON.stringify(todos.value))
        }
        return { newTodo, todos, add, finish, remove }
    }
}
</script>
<template>
    <div>
        <h1>ToDo App</h1>
        <form @submit.prevent="add()">
            <label>New ToDo </label>
            <input v-model="newTodo" name="newTodo" autocomplete="off">
            <button>Add ToDo</button>
        </form>
        <h2>ToDo List</h2>
        <ul>
            <li v-for="(todo, index) in todos" :key="index">
                <span :class="{ done: todo.done }" @click="finish(todo)">{{ todo.content }}</span>
                <button @click="remove(index)">Remove</button>
            </li>
        </ul>
        <h4 v-if="todos.length === 0">Empty list.</h4>
    </div>
</template>
<style scoped>

</style>
