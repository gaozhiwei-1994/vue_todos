<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="selectedAll"/>
    </label>
    <span>
      <span>完成{{completedSize}}</span> /全部： {{todos.length}}
    </span>
    <button class="btn btn-danger" v-show="completedSize>0" @click="removeComleted">清除已完成任务</button>
  </div>
</template>

<script>
  export default {
    //标签属性传递  接收
    //props: ['todos','removeComleted','allSelected'],
    props: ['todos'],
    //计算属性
    computed: {
      /*
      *var total = 0
       for (var i = 0; i < todos.length; i++) {
       var todo = todos[i]
       total += todo.complete ? 1 : 0
       }
      * */
      //计算选中的完成的个数
      completedSize () {
        return this.todos.reduce((preTotal,todo)=>{
          return preTotal + (todo.complete ? 1 : 0)
        },0)
      },
      selectedAll: {
        get () {
          //获取选中状态时调用
          //全部删除时，也会选中，所以 &&
          //console.log(this.completedSize === this.todos.length && this.completeSize != 0)
          return this.completedSize===this.todos.length && this.completedSize!=0
        },
        set (value) {
          //更新todos的选中 状态   标签属性接收的事件回调函数
          //this.allSelected(value)
          //调用自定义事件   效果和上一行代码一样
          this.$emit('allSelected',value)
        }
      }
    },
    methods: {
      removeComleted () {
        //调用自定义事件
        this.$emit('removeComleted')
      }
    }
  }
</script>

<style>
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
