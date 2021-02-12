<template>
  <div id="app">
    <TodoHeader/>
    <TodoInput @addTodoItem = "addOneItem"/>
    <TodoList :todoItems = "todoItems" @removeItem="removeOneItem" @toggleItem="toggleOneItem"/>
    <TodoFooter @clearAll="clearAllItems"/>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'
import TodoFooter from './components/TodoFooter.vue'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoList,
    TodoInput,
    TodoFooter
  },
  data: function(){
    return{
      todoItems: []
    }
  },
  methods: {
    addOneItem: function(todoItem){
      var obj = {completed: false, item: todoItem};
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index){
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem: function(todoItem, index){
      this.todoItems[index].completed = !this.todoItems[index].completed;
      // 로컬스토리지엔 업데이트를 바로 할 수 있는 방법이 없어서 지우고 다시 저장해줌(데이터갱신)
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
    clearAllItems: function(){
      localStorage.clear();
      this.todoItems = [];
    }
  },
   created: function() {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }
      }
    }
  }

}
</script>

<style>
body{
  text-align: center;
  background: #f6f6f6;
}
input{
  border-style: groove;
  width:200px;  
}
button{
  border-style:groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0,0,0,0.03);
}
  
</style>

