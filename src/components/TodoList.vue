<template>
  <div class="dolist">
    <div class="topLabel">
      <h1>CREATE TODO LIST</h1>
    </div>
    
    <section>
      <form>
        <input v-model="newTodo" type="text" placeholder="Add New Tasks Here" class="input-tasks"/>
        <button @click.prevent="addTodoItem" class="addTodoBtn">Add Todo</button>
      </form>
    </section>
    <section class="todo-list">
      <div class="list">
        <div v-for="(todo,index) in tasks_asc" :key="index" class="todo-item">
          <span>
            <input type="checkbox" v-model="todo.done" class="input-check"/>
          </span>

          <div class="todo-content">
            <input type="text" v-model="todo.content" :class="{done:todo.done}"/>
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        
        </div>
      </div>
    </section>
     
  </div>
</template>

<script>
import {ref,onMounted,computed,watch} from 'vue'
export default {
  name: 'TodoList',

  setup(){   
    
    const newTodo=ref('')
    const todos=ref([])
  
    const tasks_asc=computed(()=>todos.value.sort((a,b)=>{
      return b.createdAt-a.createdAt
    }))

    const removeTodo=(todo)=>{
      todos.value=todos.value.filter((t)=>t!==todo)
    }

    const addTodoItem = ()=>{
      if(newTodo.value.trim()===''){
        return
      }
      todos.value.push({
        content:newTodo.value,
        done:false,
        createdAt:new Date().getTime()
      })
      newTodo.value=''
    }

    watch(todos,newval=>{
      localStorage.setItem('todos',JSON.stringify(newval))
    },{deep:true})

    onMounted(() => {
	      todos.value = JSON.parse(localStorage.getItem('todos')) || []
    })

    return {newTodo,addTodoItem,removeTodo,todos,tasks_asc}
  }

}
</script>

