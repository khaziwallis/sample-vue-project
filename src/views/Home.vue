<template>
  <AddTask v-if="showAddTask" @add-task="addTask" />
  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>

import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
  name: 'Home',
  components: {
    Tasks, AddTask
  },
  data() {
    return {
      tasks: []
    }
  },
  props: {
    showAddTask: {
      type: Boolean
    },
    //showAddTask: Boolean
  },
  methods: {
    async deleteTask(id) {
      if (confirm('Are you sure, you want to delete Task?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
          headers: {
            'Content-Type': 'application/json'
          }
        });
        if (res.status === 200) {
          this.tasks = this.tasks.filter((task) => task.id !== id );
        } else {
          alert('Error, Deleting Task');
        }
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = {...taskToToggle, reminder: !taskToToggle.reminder };
      const response = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updateTask)
      });
      const data = await response.json();
      if (response.status === 200) {
        this.tasks = this.tasks.map((task) => {
          if (task.id == id) {
            return {
              ...task, reminder: data.reminder
            };
          }
          return task;
        });
      }
    },
    async addTask(newTask) {
      const res = await fetch(`api/tasks`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newTask)
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async fetchTasks() {
      const response = await fetch('api/tasks');
      const data = await response.json();
      return data;
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`);
      const data = await response.json();
      return data;
    }
  },
  async created() {
    this.tasks = await this.fetchTasks();
  }
}
</script>
