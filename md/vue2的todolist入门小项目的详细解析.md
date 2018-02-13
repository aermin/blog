---
title: vue2的todolist入门小项目的详细解析
type: tags
date: 2017-05-09 13:08:49
tags:
      - vue2
      - 项目总结
---
看完vue2的官方文档后，找个入门项目巩固下知识点，简单的todolsit是个不错的选择。
项目用到了`vue.js` `vue.cli` `webpack` `ES6` `node环境`，完成项目后会对这些技术栈有了些了解。

<!-- more -->

### 准备开发环境
* $ npm install -g vue-cli 
* $ vue init <template-name> <project-name> ，比如 vue init webpack todolist
* $ cd todolist
* $ npm install
* $ npm run dev
* 安装谷歌插件vue.js devtools
* 下载vue.js的[webpack模板](https://github.com/vuejs-templates/webpack)
* 下载 [todomvc的模板](https://github.com/tastejs/todomvc-app-template) (提供html和css)（也可以直接$ git clone https://github.com/tastejs/todomvc-app-template.git 来下载）
* 把todomvc的index.html拖到todolist文件夹去覆盖里面的index.html
* 打开todomvc的json文件，会看到 "todomvc-app-css": "^2.0.0",就是要你 npm  install todomvc-app-css -S 从而下载该css
* 删点todolsit index.html的默认css，js引用，src文件夹下的main.js引入模板css（import‘todomvc-app-css/index.css’）
* 引入vue（import Vue form ‘vue’）
* 等写完代码后 $npm run build 一键打包构建，会看到dist文件夹

###  main.js的代码

> //后面的为注释讲解， ~表示串联index.html的对应内容

```
import 'todomvc-app-css/index.css'

import Vue from 'vue'

//添加localStorage
var STORAGE_KEY = 'todos-vuejs-2.0'
var todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}

//用过滤器筛选出三种状态
var filters = {
  all(todos) {
    return todos
  },

  active(todos) {
    return todos.filter((todo) => {
      return !todo.completed
    })
  },

  completed(todos) {
    return todos.filter((todo) => {
      return todo.completed
    })
  },
}


let app = new Vue({
  el: '.todoapp',    // ~ <section class="todoapp">
  data: {
    msg: 'hello world',
    title: '待做清单',   // 渲染标题 ~ {{title}}
    newTodo: '',
     todos: todoStorage.fetch(),  // ~ v-show="todos.length" ； ~ {{todos.length>1?'items':'item'}} 渲染 li ~  v-for="(todo,index) in filteredTodos" 
    editedTodo: '',   // 空的编辑对象
    hashName: 'all'    
  },
  
  watch: {
    todos: {
      handler: function (todos) {
        todoStorage.save(todos)
      },
      deep: true
    }
  },
  
  computed: {
    remain() {
      return filters.active(this.todos).length   //未完成事项的数量 ~ {{remain}}
    }, 
    isAll: {     // ~ v-model="isAll"
      get() {
        return this.remain === 0
      },
      set(value) {
        this.todos.forEach((todo) => {
          todo.completed = value
        })
      }
    },
    filteredTodos() {    //用hashName过滤出当前页面的todos  ~ v-for="(todo,index) in filteredTodos" 
      return filters[this.hashName](this.todos)
    }
  },

  methods: {
    addTodo(e) {  //输入值为空时，不添加（trim去除前后空格） ~ v-model.trim="newTodo" 
      if (!this.newTodo) {
        return
      }
      this.todos.push({
        id: todoStorage.uid++,
        content: this.newTodo,
        completed: false //结合v-model 根据completed状态绑定样式  ~:class="{completed:todo.completed； ~ v-model="todo.completed"
      })
      this.newTodo = ''
    },
    removeTodo(index) {   //绑定x样式，点击删除该todo ~ @click="removeTodo(index)"
      this.todos.splice(index, 1)
    },
    editTodo(todo) {         //编辑 ~ @dblclick="editTodo(todo)"
      this.editCache = todo.content //储存编辑前的内容
      this.editedTodo = todo  // 点击编辑里面的内容而不是只是空文本框~ editing:todo==editedTodo}"
    },
    doneEdit(todo, index) {  //失去焦点后 ~ @blur="doneEdit(todo)"；@keyup.enter="doneEdit(todo)"
      this.editedTodo = null  //不存在编辑了或者说编辑已完成
      if (!todo.content) {  //如果编辑后没有内容了，删除该todo
        this.removeTodo(index)
      }

    },
    cancelEdit(todo) {    //按esc键取消此次编辑操作 ~ @keyup.esc="cancelEdit(todo)">
      this.editedTodo = null     
      todo.content = this.editCache //当esc取消编辑时，还原编辑前的内容
    },
    clear() {  //点击清除已完成的功能 ~ @click="clear"
      this.todos = filters.active(this.todos)  //获取并渲染未完成的事项 ~ 
    }
  },
  directives: {   //自定义属性 ~ v-focus="todo == editedTodo"
    focus(el, value) {  //文本框双击获取焦点
      if (value) {
        el.focus()
      }
    }
  }
})

//hash（url地址中#以及之后的字符）
function hashChange() { 
// ~ :class="{selected:hashName=='all'}"；:class="{selected:hashName=='active'}"；:class="{selected:hashName=='completed'}"
  let hashName = location.hash.replace(/#\/?/, '')  //正则表达式去除#/？，获取如all，active，completed
  if (filters[hashName]) {   //如果过滤状态的hashName存在
    app.hashName = hashName  //给整个app变量里的hashName赋上那个值
  } else {
    location.hash = ''    //取消
    app.hashName = 'all'  //否则就赋值‘all’，回到全部事项的页面
  }
}

window.addEventListener('hashchange', hashChange) //全局监听hash
```







