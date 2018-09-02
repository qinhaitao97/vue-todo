<template>
    <section class="real-app">
        <input type="text" class="add-input" autofocus @keyup.enter="addTodo" placeholder="add your plans...">
        <item :todo="todo" v-for="todo in filteredTodos" :key="todo.id" @del='deleteTodo' @done="done"/>
        <tabs :filter="filter" :todos="todos" @toggle='toggleFilter' @clearAllCompleted="clearAllCompleted"/>
     </section>
</template>

<script>
import Item from "./item.vue";
import Tabs from "./tabs.vue";

// 如果要加入 localStorage 的功能，就不能再使用全局变量了，因为每次刷新都会初始化这个 id 值，导致每次添加到数组中的对象的 id 都是0
// let id = 0;
let save = (function() {
  return {
    add(todos) {
      localStorage.setItem("todos", JSON.stringify(todos));
    },
    get() {
      return JSON.parse(localStorage.getItem("todos"));
    },
    remove() {
      localStorage.removeItem("todos");
    }
  };
})();

export default {
  data() {
    return {
      // 初始化页面时，取回 localStorage 中的信息展示
      todos: save.get() || [],
      id: save.get() ? save.get().length : 0,

      // 页面默认展示全部的 todo 事项
      filter: "all"
    };
  },
  components: {
    Item,
    Tabs
  },
  computed: {
    // 要传递给子组件 item 的数据是要经过筛选的，根据当前则的是 all、active、completed 判断
    filteredTodos() {
      if (this.filter === "all") {
        return this.todos;
      }
      // 完成就是 true，未完成就是 false，两种状态
      const completed = this.filter === "completed";
      return this.todos.filter(todo => completed === todo.completed);
    }
  },
  methods: {
    addTodo(e) {
      if (e.target.value.trim() === "") {
        alert("你不输入，我怎么知道你接下来要做什么呢？");
        e.target.value = "";
        return;
      }
      this.todos.unshift({
        id: this.id++,
        content: e.target.value.trim(), //去空格
        completed: false
      });

      // 添加数据后，更新 localStorage
      this.todos;

      // 增加 todo 结束后，将 input 清空
      e.target.value = "";
    },
    deleteTodo(id) {
      // 删除对应 id 的 item
      this.todos.splice(this.todos.findIndex(todo => todo.id === id), 1);

      // 删除后更新 localStorage
      save.add(this.todos);
    },
    toggleFilter(state) {
      this.filter = state;
    },
    clearAllCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);

      // 筛选后，更细 localStorage
      save.add(this.todos);
    },
    done(id) {
      this.todos[this.todos.findIndex(todo => todo.id === id)].completed = !this
        .todos[this.todos.findIndex(todo => todo.id === id)].completed;

      // 改变 todo 事项的状态后，更新 localStorage
      save.add(this.todos);
    }
  }
};
</script>

<style lang="stylus" scoped>
.real-app {
	width: 700px;
	margin: 0 auto;
	box-shadow: 0 0 5px #666;
}

.add-input {
	position: relative;
	margin: 0;
	width: 100%;
	font-size: 24px;
	font-family: inherit;
	font-weight: inherit;
	line-height: 1.4em;
	border: 0;
	outline: none;
	color: inherit;
	box-sizing: border-box;
	font-smoothing: antialiased;
	padding: 16px 16px 16px 36px;
	border: none;
	box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
}
</style>


