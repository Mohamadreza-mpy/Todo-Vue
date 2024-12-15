<script setup>
import {ref, onMounted, computed, watch} from "vue";

const todos = ref([]);
const name = ref('')

// input :
const input_content = ref('')
const input_category = ref(null)

// filter :
const  todo_asc = computed(() => todos.value.sort((a, b) => {
  return b.createAt - a.createAt
}))



const addTodo = ()=> {

  if(input_content.value.trim() === '' || input_category.value === null)
  {
    return
  }

  todos.value.push({
    content:input_content.value,
    category: input_category.value,
    done:false,
    createAt : new Date().getTime()
  })

}


const removeTodo = todo => {
    todos.value = todos.value.filter(t => t !== todo)
}

watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep:true})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
<!-- main tag-->
 <main class="app">


   <section class="create-todo">
      <h3>CREATE A TODO</h3>
     <form @submit.prevent="addTodo">
       <h4>what's on your todo list ?  </h4>
       <input
         type="text"
         placeholder="e.g. make video"
         v-model="input_content"/>

       <h4>Pick a category</h4>

       <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="input_category"/>
            <span class="bubble business"></span>

            <div>Business</div>


          </label>

         <label>
           <input
             type="radio"
             name="category"
             id="category1"
             value="personal"
             v-model="input_category"/>
           <span class="bubble personal"></span>

           <div>Personal</div>


         </label>

       </div>

       <input type="submit" value="Add Todo"/>
     </form>
   </section>
  <section class="todo-list">
    <h3>TODO LIST</h3>
    <div class="list">
      <div v-for="todo in todo_asc" :class="`todo-item ${todo.done && 'done'}`">

        <label>
            <input type="checkbox" v-model="todo.done"/>
            <span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"> </span>
        </label>
        <div class="todo-content">
          <input type="text" v-model="todo.content">
        </div>

        <div class="actions">
          <button class="delete" @click="removeTodo(todo)">Delete</button>
        </div>

      </div>
    </div>
  </section>
 </main>
</template>

<style scoped>

</style>
