<template>
  <div class= "container">
      <h1 class="text-center mt-5">TODO LIST</h1>
      
      <div class="d-flex">
          <input type="text" class="form-control" placeholder="Enter activity" v-model="newTodo" @keyup.enter="addTodo">
          <button class="btn btn-dark rounded-1" @click="addTodo">ENTER</button>
      </div>
      
      <transition-group enter-active-class="animated fadeInBottomLeft" leave-active-class="animated fadeOutBottomRight">
      <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
            <input type="checkbox" class="form-check-input" v-model="todo.completed">
            <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="text-center" :class="{completed:todo.completed}">
                {{todo.title}}
            </div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" 
            @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
        <div class="text-center" @click="removeTodo(index)">
            <span class="fa fa-trash"></span>
        </div>
      </div>
      </transition-group>

      <div class="extra-container">
          <div><label><input type="checkbox" class="form-check-input" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
          <div>{{remaining}} items left</div>
      </div>

      <div class="extra-container">
          <div>
              <button class="btn btn-success" :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
              <button class="btn btn-success mx-1" :class="{active: filter == 'active'}" @click="filter = 'active'">Active</button>
              <button class="btn btn-success" :class="{active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
          </div>

          <div>
              <transition name="fade">
              <button class="btn btn-danger" v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
              </transition>
          </div>
      </div>
      <figure class="text-center">
          <figcaption class="text-center"> 
              <cite class="cite-text">Double Click to edit todo item.
          By: Phila Ngubentombi</cite>
          </figcaption>
      </figure>
  </div>
</template>

<script>
export default {
  name: 'todo',
  data () {
      return {
        newTodo: '',
        idForTodo: 3,
        beforeEditCache: '',
        filter: 'all',
        todos:[
            {
            'id': 1,
            'title': 'Finish Vue Screencast',
            'completed': false,
            'editing': false,
            },
            {
            'id': 2,
            'title': 'Take over the World wide web',
            'completed': false,
            'editing': false,
            },
        ]
    }
  },
  computed:{
      remaining() {
          return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining(){
          return this.remaining != 0
      },
      todosFiltered() {
          if(this.filter == 'all'){
              return this.todos
          } else if(this.filter == 'active'){
              return this.todos.filter(todo => !todo.completed)
          }else if(this.filter == 'completed'){
              return this.todos.filter(todo => todo.completed)
          }
          return this.todos
      },
      showClearCompletedButton() {
          return this.todos.filter(todo => todo.completed).length > 0
      }
  },
  directives:{
      focus: {
          inserted: function (el) {
              el.focus()
          }
      }
  },
  methods:{
      addTodo(){
          if(this.newTodo.trim().length == 0){
              return
          }
          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false,
              editing: false,
          })

          this.newTodo = ''
          this.idForTodo++
      },
      editTodo(todo) {
          this.beforeEditCache = todo.title
          todo.editing = true
      },
      doneEdit(todo) {
          if(todo.title.trim() == ''){
              todo.title = this.beforeEditCache
              return
          }
          todo.editing = false
      },
      cancelEdit(todo) {
          todo.title = this.beforeEditCache
          todo.editing = false
      },
      removeTodo(index){
          this.todos.splice(index,1)
      },
      checkAllTodos(){
          this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted(){
          this.todos = this.todos.filter(todo => !todo.completed)
      }
  }
}
</script>

<style>
.todo-input{
        width: 100%;
        padding:10px 18px;
        font-size:18px;
        margin-bottom:16px;
        outline: 0;
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 1s;
    }

    .todo-item-left{
        display: flex;
        align-items: center;
    }

    .todo-item-lable{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc; 
        font-family: 'Avenir', Helvetica, Arial, Helvetica, sans-serif
    }

    .completed{
        text-decoration: line-through;
        color: red;
    }

    .extra-container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom:14px;
    }
    .btn-toolbar{
       padding-right: 10px; 
    }

    .active {
        background: lightgreen
    }

    .fade-enter-active {
        animation: fadeIn;
        animation-duration: 2s;
    }

    .fade-leave-active {
        animation: fadeOut;
        animation-duration: 2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

    .cite-text{
        font-size:12px;
        color: rgb(10, 46, 82)
    }

</style>