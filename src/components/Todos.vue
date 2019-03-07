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
                <li class="todo" v-for="todo in filteredTodos"
                    :class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" class="toggle" v-model="todo.completed">
                        <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit"
                           @keyup.enter="doneEdit"
                           @keyup.esc="cancelEdit"
                           @blur="doneEdit"
                           v-model="todo.name"
                           v-focus="todo === editing"
                    >
                </li>
            </ul>
        </section>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> Tâches à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}"
                       @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}"
                       @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}"
                       @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" @click.prevent="deleteCompleted" v-show="completing">
                Supprimer les tâches finies
            </button>
        </footer>
    </section>
</template>

<script>
    import Vue from 'vue'

    export default {
        name: 'Todos',
        data () {
            return {
                todos: [],
                newTodo: '',
                filter: 'all',
                editing: null
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
            completing () {
                return this.todos.filter(todo => todo.completed).length
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
                if (this.newTodo === '') return

                this.todos.push({
                    name: this.newTodo,
                    completed: false
                })
                this.newTodo = ''
                this.oldTodo = ''
            },
            editTodo (todo) {
                this.editing = todo
                this.oldTodo = todo.name
            },
            doneEdit () {
                this.editing = false
            },
            cancelEdit () {
                this.editing.name = this.oldTodo
                this.doneEdit()
            },
            deleteTodo (item) {
                this.todos = this.todos.filter(todo => todo !== item)
            },
            deleteCompleted () {
                this.todos = this.todos.filter(todo => !todo.completed)
            }
        },
        directives: {
            focus (el, value) {
                if (value) {
                    Vue.nextTick(() => el.focus())
                }
            }
        }
    }
</script>

<style src="./todos.css"></style>