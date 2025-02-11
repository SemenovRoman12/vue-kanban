<template>
  <div class="app-container">
    <h1>Моя Kanban доска</h1>
    <div class="columns-container">
      <todo-column
          :tasks="tasks.filter(t => t.status === 'todo')"
          @create-task="createTask"
          @edit-task="editTask"
          @delete-task="deleteTask"
          @move-task="moveTask"
      />

      <in-progress-column
          :tasks="tasks.filter(t => t.status === 'in-progress')"
          @edit-task="editTask"
          @move-task="moveTask"
      />

      <testing-column
          :tasks="tasks.filter(t => t.status === 'testing')"
          @edit-task="editTask"
          @move-task="moveTask"
      />

      <done-column
          :tasks="tasks.filter(t => t.status === 'done')"
      />
    </div>
  </div>
</template>

<script>
import TodoColumn from './components/TodoColumn.vue'
import InProgressColumn from './components/InProgressColumn.vue'
import TestingColumn from './components/TestingColumn.vue'
import DoneColumn from './components/DoneColumn.vue'

export default {
  name: 'App',
  components: {
    TodoColumn,
    InProgressColumn,
    TestingColumn,
    DoneColumn
  },
  data() {
    return {
      tasks: []
    }
  },
  methods: {
    createTask({ title, description, deadline }) {
      const now = new Date();
      this.tasks.push({
        id: Date.now(),
        title,
        description,
        deadline,
        dateCreated: now,
        lastEdit: now,
        status: 'todo',
        isOverdue: false,
        returnReason: null
      });
    },

    editTask({ id, title, description, deadline }) {
      const task = this.tasks.find(t => t.id === id);
      if (task) {
        task.title = title;
        task.description = description;
        task.deadline = deadline;
        task.lastEdit = new Date();
      }
    },

    deleteTask(id) {
      this.tasks = this.tasks.filter(t => t.id !== id);
    },


    moveTask({ id, newStatus, reason }) {
      const task = this.tasks.find(t => t.id === id);
      if (task) {
        if (reason) {
          task.returnReason = reason;
        }

        task.status = newStatus;
        task.lastEdit = new Date();

        if (newStatus === 'done') {
          const now = new Date();
          const deadlineDate = new Date(task.deadline);
          task.isOverdue = now > deadlineDate;
        }
      }
    }
  }
}
</script>

<style>
.app-container {
  margin: 0 auto;
  padding: 20px;
  max-width: 1200px;
}
.columns-container {
  display: flex;
  gap: 20px;
  justify-content: space-between;
}
h1 {
  text-align: center;
  margin-bottom: 20px;
}
</style>
