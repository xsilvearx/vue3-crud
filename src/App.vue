<script setup>
import { ref, watch } from 'vue'

const STORAGE_KEY = 'vue-crud-tasks'
const tasks = ref(
  
JSON.parse(localStorage.getItem(STORAGE_KEY)) || 
  [{id: Date.now(), title: 'Build CRUD app'}

  ])

watch (
  tasks,
  (newTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks))
  },
  {deep:true}
)

const newTask = ref('');

const addTask = () => {
  const trimmed = newTask.value.trim();
  if (!trimmed) return 

  tasks.value.push({
    id: Date.now(),
    title: trimmed
  });
  newTask.value= '';
} 

const editID = ref(null);
const editTitle = ref('');

function startEdit(task){
  editID.value = task.id;
  editTitle.value = task.title;
}

function saveEdit(){
  const trimmed = editTitle.value.trim();
  if (!trimmed) return

  const task = tasks.value.find(t => t.id === editID.value);
  if(task)(
    task.title = trimmed
  )

  editID.value = null;
  editTitle.value = '';
}


function cancelEdit() {
  editID.value = null;
  editTitle.value = '';
}

function deleteTask(id){
  tasks.value = tasks.value.filter(task => task.id !== id)
}
</script>

<template>
  <div>
    <h1>Task List</h1>
    <div class="input-text">
      <label for="taskTitle">
        <input 
        type="text" 
        id="taskTitle" 
        v-model="newTask"
        placeholder="Task title">
      </label>
      <button type="button" @click="addTask">ADD</button>
    </div>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        <template v-if="editID === task.id">
          <input v-model="editTitle">
          <button type="button" @click="saveEdit">SAVE</button>
          <button type="button" @click="cancelEdit">Cancel</button>
        </template>

        <template v-else>
          {{ task.title }}
          <button type="button" @click="startEdit(task)">Edit</button>
          <button type="button" @click="deleteTask(task.id)">DELETE</button>
        </template>
          
      </li>
    </ul>
  </div>
</template>

<style scoped>
h1 {
  margin-bottom: 10px;

}
.input-text {
  display: flex;
  align-items: center ;
}
</style>
