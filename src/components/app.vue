<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <todo-header :add="add"></todo-header>
      <list :todos="todos" :remove="remove"></list>
      <todo-footer :todos="todos" :all-selected="allSelected" :remove-comleted="removeComleted"></todo-footer>
    </div>
  </div>
</template>

<script>
  import header from './header.vue'
  import list from './list.vue'
  import footer from './footer.vue'
  import local from './local'

  export default {
    data () {
      return {
        //初始化数据
        todos: [
          {content: '抽烟', complete: true},
          {content: '喝酒', complete: false},
          {content: '烫头', complete: false}
        ]
      }
    },
    //实例对象创建完成
    created () {
      //去localstorage  读取
      this.todos = local.read()
    },
    methods: {
      //添加数据的方法，传递到header
      add (todo) {
        this.todos.unshift(todo)
      },
      //删除todo的方法
      remove (index) {
        this.todos.splice(index,1)
      },
      //全选函数
      allSelected (isComplete) {
        this.todos.forEach(todo =>{
          todo.complete = isComplete
        })
      },
      //清除完成todo
      removeComleted () {
        if(confirm(`确认清空吗`)){
          //过滤一个为false的新数组
          this.todos = this.todos.filter(todo => !todo.complete)
        }
      }
    },
    //深度监视todos，一旦变化，调用save
    watch: {
      todos: {
        handler: local.save,
        deep: true
      }
    },
    //将引入的组件，映射为标签
    components: {
      'todo-header': header,
      list,
      'todo-footer': footer
    }
  }
</script>

<style>
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
