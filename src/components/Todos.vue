<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tache"
                   v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <section class="main">
            <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
            <label for="toggle-all">Mark all as complete</label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed}">
                    <div class="view">
                        <input type="checkbox" class="toggle" v-model="todo.completed">
                        <label>{{ todo.name }}</label>
                    </div>
                </li>
            </ul>
        </section>
        <footer class="footer">
            <span class="todo-count"><strong>{{ remaining }}</strong> Tâches à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}"
                       @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}"
                       @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}"
                       @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
        </footer>
    </section>
</template>

<script>
    export default {
        name: 'Todos',
        data () {
            return {
                todos: [{
                    name: 'Tache de test',
                    completed: false
                }],
                newTodo: '',
                filter: 'all'
            }
        },
        computed: {
            allDone: {
                get () {
                    return this.remaining === 0
                },
                set (value) {
                    this.todos.map(todo => todo.completed = value)
                }
            },
            remaining () {
                return this.todos.filter(todo => !todo.completed).length
            },
            filteredTodos () {
                if (this.filter === 'todo') {
                    return this.todos.filter(todo => !todo.completed)
                } else if (this.filter === 'done') {
                    return this.todos.filter(todo => todo.completed)
                }

                return this.todos
            }
        },
        methods: {
            addTodo () {
                this.todos.push({
                    name: this.newTodo,
                    completed: false
                })
                this.newTodo = ''
            }
        }
    }
</script>

<style src="./todos.css"></style>