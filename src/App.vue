<template>
  <div>
    <header>
      <img src="./assets/pinia.png" alt="">
      <h1>{{ taskStore.name }}</h1>
    </header>
    <!-- new task form-->

    <div class="new-task-form">
      <TaskForm/>
    </div>

    <main>
        
      <!-- filter -->
      <nav class="filter">
        <button @click="filter = 'all'">All tasks</button>
        <button @click="filter = 'favs'">Fav tasks</button>
      </nav>

      <!-- loading -->
      <div class="loading" v-if="loading">
        Loading tasks...
      </div>


      <!-- task list-->
      <div class="task-list" v-if="filter === 'all'">
        <p>Your have {{ totalCount }} tasks left to do</p>
        <div v-for="task in tasks">
          <TaskDetails :task="task"/>
        </div>
      </div>

      <div class="task-list" v-if="filter === 'favs'">
        <p>Your have {{ favCount }} favs left to do</p>
        <div v-for="task in favs">
          <TaskDetails :task="task"/>
        </div>
      </div>
      
      <button id="reset" @click="taskStore.$reset">Reset</button>
    </main>

  </div>
</template>

<script>
import { ref } from 'vue';
import TaskDetails from './components/TaskDetails.vue';
import TaskForm from './components/TaskForm.vue';
import { useTaskStore } from './stores/TaskStore';
import { storeToRefs } from 'pinia';

  export default {
    components: { TaskDetails, TaskForm },
    setup(){
      const taskStore = useTaskStore();

      const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore)
      // fetch tasks 
      taskStore.getTasks()

      const filter = ref('all');

      return { taskStore, filter, tasks, loading, favs, totalCount, favCount }
    }
  }
</script>