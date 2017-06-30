<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <todo-header :add="add"></todo-header>
      <list :todos="todos" :remove="remove"></list>
      <!--通过属性标签传递事件 和 数据-->
      <!--<todo-footer :todos="todos" :all-selected="allSelected" :remove-comleted="removeComleted"></todo-footer>-->
      <!--通过自定义事件-->
      <todo-footer ref="footer" :todos="todos" @allSelected="allSelected"></todo-footer>
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
      //console.log(this)
      this.todos = local.read()
    },
    //为什么不在created，因为created还没有进行编译，拿不到ref，也就拿不到footer组件对象
    mounted () {
      var footer = this.$refs.footer
      //事件名称  和  对应执行的回调函数
      footer.$on('removeComleted',this.removeComleted)
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
       /* handler (newtodos) {
          local.save(newtodos)
        },*/
       //这里和上边的写法是一样的，js原理简化结构
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
