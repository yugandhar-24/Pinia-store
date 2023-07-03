<template>
  <main>
    <!-- heading -->
    <header>
    <img src="./assets/pinia-logo.svg" alt="" srcset="">
    <h1>Pinia Tasks
    </h1>
  </header>
  <!-- loading -->
  <div class="loading" v-if="isLoading">
    Loading.....
  </div>
  <!-- new task form -->
  <div class="new-task-form">
    <AddTaskForm/>
  </div>
  
  <!-- filter -->
  <div class="filter">
    <button @click="filter = 'all'">All Tasks</button>
    <button  @click="filter = 'favs'">Fav Tasks</button>
  </div>

  <!-- Tasks List -->
  <div class="task-list" v-if="filter === 'all'">
    <p>You have {{ totalCount }} tasks left to do</p>
    <div v-for="task in tasks" :key="task.id">
      <TaskDetails :task="task"/>
    </div>
  </div>


  <div class="task-list" v-if="filter === 'favs'">
    <p>You have {{ favCount }} favs left to do</p>
    <div v-for="task in favs" :key="task">
      <TaskDetails :task="task"/>
    </div>
    
  </div>
    <button @click="taskStore.$reset">Reset State</button>

  </main>
 

</template>

<script>
import { ref } from 'vue'
import TaskDetails from './components/TaskDetails.vue'
import { useTaskStore } from './stores/TaskStore'
import AddTaskForm from './components/AddTaskForm.vue'
import { storeToRefs } from 'pinia'

export default {
  components: { TaskDetails, AddTaskForm },
  setup() {
    const taskStore = useTaskStore()

    const {tasks,isLoading,favs, totalCount, favCount}=storeToRefs(taskStore)

    // fetch tasks
    taskStore.getTasks()
    const filter=ref('all')

    return {taskStore,filter, tasks,isLoading,favs, totalCount, favCount}
  }
}
</script>