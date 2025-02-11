<template>
  <div class="column-container">
    <h2>Задачи в работе</h2>

    <div v-for="task in tasks" :key="task.id" class="task-card">
      <h3>{{ task.title }}</h3>
      <p><strong>Описание:</strong> {{ task.description }}</p>
      <p><strong>Дата создания:</strong> {{ formatDate(task.dateCreated) }}</p>
      <p><strong>Дэдлайн:</strong> {{ formatDate(task.deadline) }}</p>
      <p><strong>Последнее редактирование:</strong> {{ formatDate(task.lastEdit) }}</p>

      <button @click="onEditTask(task)">Редактировать</button>
      <button @click="onMoveTask(task)">Переместить в 'Тестирование'</button>
    </div>

    <div v-if="editingTask" class="modal-backdrop">
      <div class="modal">
        <h3>Редактировать задачу</h3>
        <input v-model="editTitle" type="text" placeholder="Заголовок" />
        <textarea v-model="editDescription" placeholder="Описание"></textarea>
        <input v-model="editDeadline" type="date" />

        <button @click="saveEdit">Сохранить</button>
        <button @click="cancelEdit">Отмена</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InProgressColumn',
  props: {
    tasks: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      editingTask: null,
      editTitle: '',
      editDescription: '',
      editDeadline: ''
    }
  },
  methods: {
    formatDate(date) {
      if (!date) return '';
      return new Date(date).toLocaleString();
    },
    formatDateForInput(date) {
      const d = new Date(date);
      const year = d.getFullYear();
      let month = (d.getMonth() + 1).toString().padStart(2, '0');
      let day = d.getDate().toString().padStart(2, '0');
      return `${year}-${month}-${day}`;
    },
    onEditTask(task) {
      this.editingTask = { ...task };
      this.editTitle = task.title;
      this.editDescription = task.description;
      this.editDeadline = this.formatDateForInput(task.deadline);
    },
    saveEdit() {
      this.$emit('edit-task', {
        id: this.editingTask.id,
        title: this.editTitle,
        description: this.editDescription,
        deadline: this.editDeadline
      });
      this.editingTask = null;
    },
    cancelEdit() {
      this.editingTask = null;
    },
    onMoveTask(task) {
      this.$emit('move-task', {
        id: task.id,
        newStatus: 'testing'
      });
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

button {
  margin-right: 5px;
  cursor: pointer;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,0.5);
}

.modal {
  background-color: #fff;
  width: 300px;
  padding: 20px;
  margin: 100px auto;
  border-radius: 4px;
}
</style>
