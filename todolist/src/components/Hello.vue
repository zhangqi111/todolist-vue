<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
   	<input type="text" v-model="m"/>
   	<input type="button" value="新增" v-on:click="addItem">
   	<input type="button" value="查找" v-on:click="filterItem">
    <ul>
     <li v-for="(item,index) in todolists" :class="{editing : editTodo == item}">
     	<input type="checkbox" name="" id="" v-bind:checked="item.done" @click="changeDone(index)"/>
     	<!--<span @click="updateItem(index)" v-if="item.flag">{{item.name}}</span>
     	<input type="text" v-if="!item.flag" @blur="completeItem" v-bind:data-index="index" @keyup.13="completeItem" v-model="item.name"/>-->
     	<span @click="updateItem(item)" class="none">{{item.name}}</span>
     	<input type="text" @blur="completeItem" @keyup.13="completeItem" v-model="item.name" class="edit" v-todo-focus=" editTodo == item "/>
     	<input type="button" value="删除" @click="todolists.splice(index,1)"/>
     </li>
    </ul>
  </div>
</template>

<script>
var STORAGE_KEY = 'todolist-vue-2.0'
var todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItems(STORAGE_KEY) || '[]')
    todos.forEach(function (item, index) {
      item.id = index
    })
  },
  save: function (item) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(item))
  }
}
export default {
  name: 'hello',
  data () {
    return {
      msg: 'todolist',
      todolists: [ {name: '张琪', done: true, flag: true},
      {name: '小亮', done: true, flag: true},
      {name: '李飞', done: true, flag: true},
      {name: '菲菲', done: true, flag: true} ],
      list: [ {name: '张琪', done: true, flag: true},
      {name: '小亮', done: true, flag: true},
      {name: '李飞', done: true, flag: true},
      {name: '菲菲', done: true, flag: true} ],
      m: '',
      editTodo: null
    }
  },
  methods: {
    addItem: function () {
      var obj = {}
      obj.name = this.m
      obj.done = false
      obj.flag = true
      this.todolists.push(obj)
      todoStorage.save(obj)
      this.m = ''
      this.list = this.todolists
    },
//  updateItem: function (index) {
//    this.todolists[index].flag = false
//    this.list = this.todolists
//  },
    updateItem: function (item) {
      this.editTodo = item
    },
//  completeItem: function (e) {
//    var newName = e.target.value
//    var index = e.target.getAttribute('data-index')
//    this.todolists[index].name = newName
//    this.todolists[index].flag = true
//    this.list = this.todolists
//  },
    completeItem: function (e) {
      this.editTodo = null
    },
    filterItem: function () {
      this.todolists = this.list
      var name = this.m
      var arr = []
      console.log(this.todolists)
      for (var i = 0; i < this.todolists.length; i++) {
        if (name === this.todolists[i].name) {
          arr.push(this.todolists[i])
        }
      }
      this.todolists = arr
    },
    changeDone: function (index) {
      this.todolists[index].done = !this.todolists[index].done
    }
  },
  directives: {
    'todo-focus': function (e, binding) {
      // e元素本身binding  自定义指定的对象   自身拥有value  默认为false
      if (binding.value) {
        e.focus()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}
ul li.editing .edit{
	display: block;
}
ul li.editing{
	display: flex;
	justify-content: center;
	align-items: center;
}
.none{
	display: inline-block;
}
ul li.editing .none{
	display:none
}
li {
  display: block;
  margin: 0 10px;
}
.edit{
	display: none;
}
a {
  color: #42b983;
}
</style>
