<template>
  <div class="container">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1">待辦事項</span>
            </div>
            <input type="text" class="form-control" placeholder="準備要做的任務" v-model="newTodo" @keyup.enter="addTodo">
            <div class="input-group-append">
            <button class="btn btn-primary" type="button" @click="addTodo" >新增</button>
            </div>
        </div>
        <div class="card text">
            <div class="card-header">
            <ul class="nav nav-tabs card-header-tabs">
                <li class="nav-item">
                <a class="nav-link" href="#" :class="{'active':visibility=='all'}" @click="visibility = 'all'">全部</a>
                </li>
                <li class="nav-item">
                <a class="nav-link " href="#" :class="{'active':visibility=='active'}" @click="visibility ='active'">進行中</a>
                </li>
                <li class="nav-item">
                <a class="nav-link" href="#" :class="{'active':visibility=='completed'}" @click="visibility = 'completed'">已完成</a>
                </li>
            </ul>
            </div>
            <ul class="list-group list-group-flush text-left">
            <li class="list-group-item" v-for="(item,key) in filteredTodo"  @dblclick="editTodo(item)">
                <div class="d-flex" v-if="item.id!==cacheTodo.id">
                <div class="form-check">
                    <input type="checkbox" class="form-check-input" id="a1" v-model="item.completed" :id="item.id">
                    <label class="form-check-label" for="a1" :class="{'completed':item.completed}" @click="item.completed" :for="item.id">
                    {{ item.title }}
                    </label>
                    
                </div>
                
                <button type="button" class="close ml-auto" aria-label="Close" @click="removeTodo">
                    <span aria-hidden="true">&times;</span>
                </button>
                </div>
                <input type="text" 
                    class="form-control" 
                    v-if="item.id===cacheTodo.id"
                    v-model="cacheTitle"
                    @keyup.esc="cancleEdit"
                    @keyup.enter="doneEdit(item)"
                    >
            </li>
      
            <li class="list-group-item">
        <!--         <input type="text" class="form-control"> -->
            </li>
            </ul>
            <div class="card-footer d-flex justify-content-between">
            <span>還有 {{countTodo}} 筆任務未完成</span>
            <a href="#" @click="deleteTodo">清除所有任務</a>
            </div>
        </div>
    </div>
</template>

<script>
export default {
      data() {
          return{
                todos: 
            [
                {
            id: '345',
            title: 'Hello',
            completed: false
                }
            ],
            newTodo: '',
            visibility: 'all',
            cacheTodo: {},
            cacheTitle: ''
          }
        
    },
    methods: {
        addTodo: function(){
        var value = this.newTodo.trim();
        var timestamp = Math.floor(Date.now());
        if (!value){
            return
        }
        this.todos.push({
            id: timestamp,
            title: value,
            completed: false
        });this.newTodo= ''
        },
        
        removeTodo: function(key){
        this.todos.splice(key,1);
        // console.log(key)
        },
        editTodo: function(item){
        // console.log(item);
        this.cacheTodo=item;
        this.cacheTitle=item.title
        },
        cancleEdit: function(){
        this.cacheTodo={}
        },
        doneEdit: function(item){
        item.title=this.cacheTitle;
        this.cacheTitle='';
        this.cacheTodo={}
        },
        deleteTodo: function(){
        this.todos = [];
        number = 1;
        }
    },
    computed: {
        filteredTodo: function(){
        if (this.visibility=='all'){
            return this.todos;
        }
        else if (this.visibility == 'active'){
            var newTodos = [];
            this.todos.forEach(function(item){
            if (!item.completed){
                newTodos.push(item);
            }
            })
            return newTodos;
            }
        else if (this.visibility == 'completed'){
            var newTodos = [];
            this.todos.forEach(function(item){
            if (item.completed){
                newTodos.push(item);
            }
            })
            return newTodos;
            }
        
        
        
        },
        countTodo: function(){
        var filteredItem = this.todos.filter(function(item){
        return (!item.completed)
        });
        console.log(filteredItem);
        
        return filteredItem.length
    
        
    }
    
    
  }
}
</script>
<style>
    .completed {
        text-decoration: line-through
        }
</style>