<template>
  <div id="app">
    <MyHeader @addTodo="addTodo"/>
    <MyList :todos="todos"/>
    <MyFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
  </div>
</template>

<script>
import pubsub from 'pubsub-js'
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'
import MyFooter from './components/MyFooter.vue'
// import { nanoid } from 'nanoid'

export default {
  name: 'App',
  components: {
    MyHeader,
    MyList,
    MyFooter
  },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || [],
      // [
      //   { id: nanoid(), title: '抽烟', done: true },
      //   { id: nanoid(), title: '喝酒', done: false },
      //   { id: nanoid(), title: '开车', done: true },
      // ],
    }
  },
  watch: {
    todos: {
      deep:true,
      handler(value){
        localStorage.setItem('todos', JSON.stringify(value));
      },
    },
  },
  methods: {
    // 增加一条todo
    addTodo(todoObj){
      this.todos.unshift(todoObj)
    },
    // 勾选/取消勾选todo
    checkTodo(id){
      this.todos.forEach(item=>{
        if(item.id === id){
          item.done = !item.done
        }
      })
    },
    // 删除一个todo
    deleteTodo(_,id){
      this.todos = this.todos.filter(item=>item.id !== id)
    },
    // 更新一个todo
    updateTodo(id, title){
      this.todos.forEach(item => {
        if (item.id === id) {
          item.title = title
        }
      })
    },
    // 全选/取消全选
    checkAllTodo(done){
      this.todos.forEach(item=>item.done=done)
    },
    // 清除已完成的todo
    clearAllTodo(){
      this.todos = this.todos.filter(item=>!item.done)
    },
  },
  mounted(){
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
    this.deleteId = pubsub.subscribe('deleteTodo', this.deleteTodo)
  },
  beforeDestroy(){
    this.$bus.$off(['checkTodo','deleteTodo','updateTodo'])
    pubsub.unsubscribe(this.deleteId);
  },
}
</script>

<style>
  /*base*/
  body {
    background: #fff;
  }

  .btn {
    display: inline-block;
    padding: 4px 12px;
    margin-bottom: 0;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
    border-radius: 4px;
  }

  .btn-danger {
    color: #fff;
    background-color: #da4f49;
    border: 1px solid #bd362f;
  }

  .btn-danger:hover {
    color: #fff;
    background-color: #bd362f;
  }

  .btn-edit {
    color: #fff;
    background-color: skyblue;
    border: 1px solid rgb(103, 159, 181);
    margin-right: 10px;
  }

  .btn-edit:hover {
    color: #fff;
    background-color: skyblue;
  }

  .btn:focus {
    outline: none;
  }

  .todo-container {
    width: 600px;
    margin: 0 auto;
  }
  .todo-container .todo-wrap {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
</style>
