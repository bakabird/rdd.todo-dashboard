<template>
  <div>
    <div class='bubble' v-if='!show' @click="show = true">
      {{ambition}}
    </div>
    <div class='todoList' v-else>
      <div class='todoItem' 
        v-for="(todo,idx) in todos"  :key="`${idx}${todo.task}`"
        :class="{'todoItem-finished': todo.finished }" 
        @click="todo.finished = !todo.finished">
        {{todo.task}}
          <input class='del' type="button" value="X" @click="removeItem(idx)"/>
        </div>
      <div class='buttons'>
        <input type="button" value='关闭' @click="show = false">
        <input type="type" v-model="newTask" @keyup.enter="addItem" v-if='startAdd'>
        <input type="button" value='添加' v-else @click="startAdd = true">
      </div>
    </div>
  </div>
</template>

<script>
import store from "store"

export default {
  props:{
    ambition: {
      type: String,
      required: true
    }
  },
  data(){
    return {
      show: true,
      startAdd: false,
      newTask: "",
      todos:[
        {
          finished: false,
          task: 'an todo items'
        }
      ]
    }
  },
  methods:{
    removeItem(idx){
      this.todos = this.todos.filter( (a,i) => i != idx)
    },
    addItem(){
      const newTodo = {
        task: this.newTask,
        finished: false
      }
      this.todos.push(newTodo)
      this.startAdd = false
    }
  },
  mounted(){
    this.todos = store.get(`$rdd_$todos_$${this.ambition}`) ? store.get(`$rdd_$todos_$${this.ambition}`) : this.todos
    window.onunload = ()=>{
      store.set(`$rdd_$todos_$${this.ambition}`, this.todos)
    }
  }
}
</script>

<style scoped>
.bubble{
  position: fixed;
  top: 5px;
  right: 5px;
  /* position */
  width: 50px;
  height: 50px;
  border-radius: 50px;
  /* shape */
  background: red;
  opacity: .25;
  /* color */
  line-height: 50px;
  color: greenyellow;
  text-align: center
  /* font */
}
.todoList{
  position: fixed;
  top: 20px;
  /* pos */
  height: 90vh;
  overflow: auto;
  /* shape */
}
.todoItem{
  background: #232624;
  /* color */
  margin-bottom: 10px;
  /* pos */
  height: 30px;
  line-height: 30px;
  padding: 2px 3px;
  /* shape */
  color: #fff;
  font-size: 16px;
  /* font */
}
.todoItem-finished{
  text-decoration: line-through;
  color: #6f7d74;
  font-size: 14px;
  /* font */
  line-height: 20px;
  height: 20px;
  /* shape */
}
.todoItem-finished .del{
  font-size: 12px;
  opacity: .5;
}
.del{
  border: none;
  /* shape */
  background: none;
  color: greenyellow;
}
.del:hover{
  color: red;
}
</style>
