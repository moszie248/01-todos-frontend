<template>
  <div id="sorting">

    <div v-for="(todo, index) in todos" :key="todo.title" v-if="showTodosVisi(todo)">
      <b-field class="is-pulled-left handle">
        <b-checkbox size="is-large" v-model="todo.completed" @input="saveTodos()">
          <strike v-if="todo.completed">{{ todo.title }}</strike>
          <span v-else>{{ todo.title }}</span>
        </b-checkbox>
      </b-field>
      <a class="delete is-pulled-right" @click="removeTodo(index)"></a>
      <div class="is-clearfix"></div>
    </div>
        <div class="is-pulled-right" v-show="countItemComplete > 0">
      <a @click="removeCompletedTodos()">Clear Completed</a>
    </div>
    <br>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import Sortable from 'sortablejs'
export default {
  data () {
    return {
    }
  },
  mounted () {
    let table = document.getElementById('sorting')
    Sortable.create(table, { handle: '.handle', onUpdate: this.onUpdate, animation: 100 })
  },
  methods: {
    ...mapActions(['removeTodo',
      'removeCompletedTodos',
      'saveTodos',
      'sortTable'
    ]),
    showTodosVisi (todo) {
      if (this.visibility !== 'all') {
        if (todo.completed === true && this.visibility === 'completed') {
          return true
        } else if (todo.completed === false && this.visibility === 'active') {
          return true
        } else return false
      } else return true
    },
    onUpdate: function (event) {
      this.sortTable({ newIndex: event.newIndex, oldIndex: event.oldIndex })
    }
  },
  computed: {
    ...mapGetters(['todos',
      'visibility'
    ]),
    countItemComplete () {
      let count = 0
      for (let i = 0; this.todos && i < this.todos.length; i++) {
        if (this.todos[i].completed) {
          count++
        }
      }
      return count
    }
  }
}
</script>