<template>
  <li>
      <label>
      <input type="checkbox" :checked="todo.done" @change="checkItem(todo.id)"/>
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input v-show="todo.isEdit" ref="inputTitle" type="text" :value="todo.title" @blur="handlerBlur(todo, $event)">
      </label>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
      <button class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
  </li>
</template>

<script>
import PubSub from 'pubsub-js';

export default {
  name: 'MyItem',
  props: ['todo'],
  methods: {
    handleDelete(id){
      if(confirm('确定删除吗？')){
        // this.deleteTodo(id) // (父传过来的)函数名
        // this.$bus.$emit('deleteTodo',id) // 全局事件总线(名)
        PubSub.publish('deleteTodo',id)  // (发布的)消息名
      }
    },
    handleEdit(todo){
      if(todo.hasOwnProperty.call(todo, 'isEdit')){
        todo.isEdit = true
      }else{
        this.$set(todo,'isEdit',true)
      }
      this.$nextTick(()=>{
        this.$refs.inputTitle.focus()
      })
    },
    handlerBlur(todo, e){
      console.log(e);
      todo.isEdit = false
      if(!e.target.value.trim()){
        alert('输入内容不能为空')
        return 
      }
      this.$bus.$emit('updateTodo',todo.id, e.target.value)
    },
    checkItem(id){
      this.$bus.$emit('checkTodo',id);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  /*item*/
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }

  li label {
    float: left;
    cursor: pointer;
  }

  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }

  li button {
    float: right;
    display: none;
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }
  li:hover {
    background-color: #EEE;
  }
  li:hover button {
    display: block;
  }
</style>
