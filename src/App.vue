<template>
  <v-container fluid>
    <v-row class="justify-center">
      <v-col cols="12" sm="10" md="8" lg="6">
        <Task
          :tasks="tasks"
          @add-task="addTask"
          @remove-task="removeTask"
          @clear-completed="clearCompleted"
          @clear-all="clearAll"
          @update-task="updateTask"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Task from '../src/components/Task.vue';

export default {
  components: { Task },
  data() {
    return {
      tasks: this.loadTasks() // Initialize tasks from local storage
    };
  },
  methods: {
    addTask(task) {
      this.tasks.push({ ...task, id: Date.now() });
      this.saveTasks();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !task.completed);
      this.saveTasks();
    },
    clearAll() {
      this.tasks = [];
      this.saveTasks();
    },
    updateTask({ index, updatedTask }) {
      this.$set(this.tasks, index, updatedTask);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const savedTasks = localStorage.getItem('tasks');
      return savedTasks ? JSON.parse(savedTasks) : [];
    }
  }
};
</script>