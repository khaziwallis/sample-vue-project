<template>
  <header>
    <h1>{{title}}</h1>
    <Button
      v-show="homePage"
      @btn-click="toggleAddTask"
      :text="showAddTask ? 'close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
    />
  </header>
</template>

<script>
import Button from './Button';
export default {
  name: 'Header',
  props: {
    title: {
      type: String,
      default: 'Task Tracker Default'
    },
    showAddTask: {
      type: Boolean
    }
  },
  components: {
    Button
  },
  methods: {
    toggleAddTask() {
      this.$emit('toggle-add-task')
    }
  },
  computed: {
    homePage() {
      if(this.$route.path === '/about') {
        return false;
      }
    }
  }
}
</script>
<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20;
  }
</style>