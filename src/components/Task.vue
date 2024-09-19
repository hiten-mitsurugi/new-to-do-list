<template>
    <v-card class="pa-4" style="max-width: 500px; margin: 50px auto 0 auto;">
      <!-- title -->
      <v-card-title>
        <h1>To Do List</h1>
      </v-card-title>
      
      <!-- form -->
      <v-form>
        <v-text-field
          v-model="newTask"
          label="New Task"
          outlined
        />
        <v-btn @click="addTask" color="primary">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-form>
  
      <!-- task table -->
      <v-data-table
        :headers="headers"
        :items="tasks"
        item-key="id"
        class="elevation-1 mt-4"
        hide-default-footer
      >
        <template v-slot:item="{ item }">
          <v-data-table-item>
            <v-row class="align-center" no-gutters>
              <v-col class="d-flex align-center">
                <v-checkbox
                  v-model="item.completed"
                  @change="toggleComplete(tasks.indexOf(item))"
                ></v-checkbox>
                <span class="ml-2">{{ item.title }}</span>
              </v-col>
              <v-col class="d-flex justify-end">
                <v-btn @click="removeTask(tasks.indexOf(item))" icon>
                  <v-icon>mdi-trash-can</v-icon>
                </v-btn>
              </v-col>
            </v-row>
          </v-data-table-item>
        </template>
      </v-data-table>
  
      <!-- buttons -->
      <v-row class="mt-4">
        <v-col>
          <v-btn @click="clearCompleted" color="error">Clear completed</v-btn>
        </v-col>
        <v-space></v-space>
        <v-col>
          <v-btn @click="clearAll" color="secondary">Clear all</v-btn>
        </v-col>
      </v-row>
  
      <!-- pending task -->
      <v-card-subtitle>
        <span>Pending Tasks: {{ pendingTasksCount }}</span>
      </v-card-subtitle>
    </v-card>
  </template>
  
  <script>
  export default {
    name: "Task",
    props: {
      tasks: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        newTask: '',
        headers: [
          { text: 'Task', value: 'title' },
          { text: 'Completed', value: 'completed', sortable: false },
          { text: 'Actions', value: 'actions', sortable: false }
        ]
      };
    },
    computed: {
      pendingTasksCount() {
        return this.tasks.filter(task => !task.completed).length;
      }
    },
    methods: {
      addTask() {
        if (this.newTask.trim()) {
          this.$emit('add-task', { title: this.newTask, completed: false });
          this.newTask = '';
        }
      },
      removeTask(index) {
        this.$emit('remove-task', index);
      },
      clearCompleted() {
        this.$emit('clear-completed');
      },
      clearAll() {
        this.$emit('clear-all');
      },
      toggleComplete(index) {
        const updatedTask = { ...this.tasks[index], completed: !this.tasks[index].completed };
        this.$emit('update-task', { index, updatedTask });
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add any additional styles if needed */
  .v-data-table-item {
    padding: 0;
  }
  </style>  