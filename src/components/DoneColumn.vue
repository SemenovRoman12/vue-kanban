<template>
  <div class="column-container">
    <h2>Выполненные задачи</h2>

    <div v-for="task in tasks" :key="task.id" class="task-card">
      <h3>{{ task.title }}</h3>
      <p><strong>Описание:</strong> {{ task.description }}</p>
      <p><strong>Дата создания:</strong> {{ formatDate(task.dateCreated) }}</p>
      <p><strong>Дэдлайн:</strong> {{ formatDate(task.deadline) }}</p>
      <p><strong>Последнее редактирование:</strong> {{ formatDate(task.lastEdit) }}</p>

      <p>
        <strong>Статус:</strong>
        <span v-if="task.isOverdue">Просрочена</span>
        <span v-else>Выполнена в срок</span>
      </p>

      <p v-if="task.returnReason">
        <strong>Причина возврата (если была):</strong> {{ task.returnReason }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DoneColumn',
  props: {
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
    formatDate(date) {
      if (!date) return '';
      return new Date(date).toLocaleString();
    }
  }
}
</script>

<style scoped>
.column-container {
  background-color: #f1f1f1;
  border-radius: 4px;
  padding: 10px;
  flex: 1;
}

.task-card {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
}

.task-card + .task-card {
  margin-top: 10px;
}
</style>
