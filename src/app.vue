<template>
  <div id="app">
    <f7-statusbar></f7-statusbar>
    <f7-views>
      <f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
        <f7-navbar v-if="$theme.ios">
          <f7-nav-center sliding>Vuex + F7</f7-nav-center>
        </f7-navbar>
        <f7-pages>
          <f7-page>
            <f7-block>
              <f7-list class="new-todo-input">
                <f7-list-item>
                  <f7-input v-on:keypress="addTodo" v-model="newTodoTitle" type="text" placeholder="Add new todo" />
                </f7-list-item>
              </f7-list>
              <f7-button v-on:click="addTodo">Add todo</f7-button>
            </f7-block>
            <f7-block-title>Today's todos</f7-block-title>
            <f7-list>
              <f7-list-item v-for="todo in todos" :title="todo.title"></f7-list-item>
            </f7-list>
          </f7-page>
        </f7-pages>
      </f7-view>
    </f7-views>
  </div>
</template>

<script>
import store from './store';

export default {
  data() {
    return {
      newTodoTitle: '',
      todos: store.getters.getTodos,
    };
  },
  methods: {
    async addTodo(e) {
      if (e && e.type === 'keypress' && e.keyCode === 13 || e.type === 'click') {
        window.f7.showPreloader();
        await store.dispatch('addTodo', {
          title: this.newTodoTitle,
        });
        window.f7.hidePreloader();
      }
    },
  },
};
</script>

<style scoped>
  .new-todo-input {
    margin-bottom: 0;
  }
</style>