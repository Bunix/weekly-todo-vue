<template>
  <div class='weekly'>
    <h1 class='centered'>{{ msg }}</h1>
    <div class='ui stackable very relaxed grid container'>
      <div class="row">
        <div class='sixteen wide column'>
          <current-week></current-week>
        </div>
      </div>
      <div class='row' v-if='isLoading'>
        <div class='sixteen wide column'>
          Loading...
        </div>
      </div>
      <div class='row' v-if='!isLoading'>
        <div class='sixteen wide column'>
          <todo-filter-menu v-bind:projects='projects'></todo-filter-menu>
          <todo-list
            @add-todo='addTodo'
            v-bind:project='project'></todo-list>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapState } from 'vuex';

// import * as db from '@/database';
// import '@/database/seed';

import { titleize } from '@/utils/strings';
import CurrentWeek from '@/components/CurrentWeek';
import TodoList from '@/components/TodoList';
import TodoFilterMenu from '@/components/TodoFilterMenu';

export default {
  name: 'Home',
  props: ['project'],

  components: {
    CurrentWeek,
    TodoList,
    TodoFilterMenu,
  },
  data() {
    return {
      msg: 'Welcome to Your Week',
    };
  },
  created() {
    this.fetchTodos();
    // db.onChange(() => this.fetchTodos());
  },
  // watch: {
  //   $route: 'fetchTodos',
  // },
  computed: {
    ...mapState({
      isLoading: state => state.todos.isLoading,
    }),
    ...mapGetters([
      'now',
      'projects',
    ]),
  },

  methods: {
    addTodo(todo) {
      const project = titleize(this.project);
      this.$store.dispatch('addTodo', { ...todo, project });
    },

    fetchTodos() {
      this.$store.dispatch('fetchTodos', { now: this.now });
    },
  },
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style>
h1, h2 {
  font-family: 'Signika', sans-serif;
}

h1 {
  margin-bottom: 2rem;
  text-align: center;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
