<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)
const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))
watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})
watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})
const addTodo = () => {
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}
	todos.value.push({
		content: input_content.value,
		category: input_category.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}
const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}
onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
	<main class="app">

    <div class="mt-[2rem] mb-[3rem]  ml-[30rem] h-[75rem] w-[34rem] bg-[#f3d5b5] text-black rounded-2xl">
      <h2 class="font-bold text-lg text-center p-4">
				To Do List
			</h2>

      <section class="flex justify-center">
        <h2 class="font-bold pl-[4rem] pt-[3rem] pb-[3rem] w-[100%] text-2xl h-[20rem] bg-[#c1121f] text-white rounded-b-3xl">
          Complete this Task<br/>for a better life
        </h2>

        <div class="absolute">
            <div class="rounded-2xl mt-[10.3rem] w-[30rem] shadow-2xl">
              <section>
                <h3 class="bg-[#ee964b] rounded-t-2xl text-center p-4 font-extrabold text-base">CREATE A TASK</h3>
            <div class="bg-[#B7B78A] rounded-b-2xl">
              <form id="new-todo-form" @submit.prevent="addTodo">
                <div class="pt-[3rem] pl-[3rem] ">
                  <h4 class="font-bold pb-2">What's on your todo list?</h4>
				          <input 
                  class="rounded-xl p-3 w-[25rem] bg-[#f3d5b5]"
					        type="text" 
					        name="content" 
					        id="content" 
					        placeholder="Enter Task"
					        v-model="input_content" />
                </div>
				
				        <h4 class="pt-8 text-center font-bold text-base">Pick a category</h4>
			        <div class="grid grid-rows-1 grid-cols-2 h-[10rem] text-center p-8">
					    <label class="bg-[#ffb700] ml-[3rem] mr-5 p-5 rounded-xl">
						    <input class="w-4 h-4  "
							  type="radio" 
							  name="category" 
							  id="category1" 
							  value="business"
							  v-model="input_category" />
						    <span class="bubble business"></span>
						  <div>Business</div>
					  </label>

					    <label  class="bg-[#168aad] ml-[3rem] mr-5 p-5 rounded-xl">
						    <input class="w-4 h-4"
							  type="radio" 
							  name="category" 
							  id="category2" 
							  value="personal"
							  v-model="input_category" />
						    <span class="bubble personal"></span>
						  <div>Personal</div>
					    </label>
				    </div>

          <button class="ml-[7rem] mb-[3rem] mt-3 p-3 bg-[#772f1a] text-white font-bold rounded-xl w-[15rem]" type="submit">Add task</button>
			</form>

            </div>
            
              </section>
            

            </div>
        </div>
		</section>


    <section class="todo-list mt-[15rem] h-[37rem] bg-white rounded-3xl">
			<h3 class="pt-[9rem] pl-[2rem] text-xl font-bold">MY TASK</h3>
			<div class="list" id="todo-list">

				<div class="ml-[3rem] mr-[3rem]" v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
						<span :class="`bubble ${
							todo.category == 'business' 
								? 'business' 
								: 'personal'
						}`"></span>
					</label>

					<div class="todo-content">
						<input type="text" v-model="todo.content" />
					</div>

					<div class="actions">
						<button class="delete bg-red-600" @click="removeTodo(todo)">Delete</button>
					</div>
				</div>

			</div>
		</section>

    </div>
		
	</main>
</template>