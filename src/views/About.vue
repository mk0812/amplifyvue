<template>
  <div>
    <nav class="level">
      <div class="level-item has-text-centered">
        <div>
          <p class="heading">TODO</p>
          <b-field label="Name">
            <b-input v-model="input.name"></b-input>
          </b-field>
        </div>
      </div>
      <div class="level-item has-text-centered">
        <div>
          <b-field label="Description">
            <b-input v-model="input.description"></b-input>
          </b-field>
        </div>
      </div>
    </nav>
    <button class="button is-link" @click="createTodo">ADD</button>
    <ul id="todo">
      <li v-for="todo in todos" :key="todo.name">
        <span class="todo-wrapper">{{ todo.name }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import {API, graphqlOperation} from 'aws-amplify'
import * as mutations from '@/graphql/mutations'
import * as queries from '@/graphql/queries'

export default {
  name: 'about',
    data: function () {
    return {
      input: {
        name: '',
        description: ''
      },
      todos: []
    }
  },
  created: async function () {
    await this.listTodos()

  },
  methods: {
    createTodo: async function () {
      if (this.input.name !== '' || this.input.description !== '') {
        await API.graphql(graphqlOperation(mutations.createTodo, {input: this.input}))
        await this.listTodos()
      }
    },
    listTodos: async function () {
      const res = await API.graphql(graphqlOperation(queries.listTodos))
      this.todos = res.data.listTodos.items
    }
  }
}
</script>
<style scoped>
.todo-wrapper {
  /* 適宜記述 */
}
</style>