<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
      <input type="checkbox" v-model="isAllTodo"/>
    </label>
    <span>
      <span>已完成{{ doneTodo }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>

export default {
  name: 'MyFooter',
  props: ['todos', 'checkAllTodo','clearAllTodo'],
  computed: {
    doneTodo(){
      return this.todos.reduce((pre,todo)=>pre+(todo.done?1:0),0)
    },
    total(){
      return this.todos.length
    },
    // isAll(){
    //   return this.doneTodo === this.total && this.total>0
    // },
    isAllTodo: {
      get(){
        return this.doneTodo === this.total && this.total > 0
      },
      set(value){
          this.checkAllTodo(value)
          return value
      },
      
    },
  },
  methods: {
    // checkAll(e){
    //   console.log(e.target.checked);
    //   this.checkAllTodo(e.target.checked)
    // },
    clearAll(){
      if(confirm('确定删除吗？')){
        this.clearAllTodo()
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  /*footer*/
  .todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
  }

  .todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }

  .todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
  }

  .todo-footer button {
    float: right;
    margin-top: 5px;
  }
</style>
