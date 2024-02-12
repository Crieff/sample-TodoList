<template>
  <div class="todo-container">
    <div class="add-task">
      <input
        v-model="newTask"
        class="add-task-input"
        placeholder="Новая задача..."
      />
      <button @click="addTask" class="add-task-button">Добавить</button>
    </div>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        {{ task.title }}
        <button @click="removeTask(task.id)" class="remove-task-button">
          ✕
        </button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { ITask } from "@/interfaces/ITask";

export default defineComponent({
  name: "TodoList",
  setup() {
    const tasks = ref<ITask[]>([]);
    let taskId = 0;
    const newTask = ref("");

    const addTask = () => {
      if (newTask.value.trim()) {
        tasks.value.push({
          id: taskId++,
          title: newTask.value,
          completed: false,
        });
        newTask.value = "";
      }
    };

    const removeTask = (id: number) => {
      tasks.value = tasks.value.filter((task) => task.id !== id);
    };

    return {
      tasks,
      newTask,
      addTask,
      removeTask,
    };
  },
});
</script>

<style>
.todo-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.add-task {
  display: flex;
  margin-bottom: 20px;
}

.add-task-input {
  flex-grow: 1;
  padding: 10px;
  border: 2px solid #ddd;
  border-radius: 4px 0 0 4px;
  outline: none;
}

.add-task-button {
  padding: 10px 20px;
  border: none;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  border-radius: 0 4px 4px 0;
  transition: background-color 0.3s;
}

.add-task-button:hover {
  background-color: #0056b3;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.remove-task-button {
  border: none;
  background-color: transparent;
  cursor: pointer;
  color: #ff6347;
}
</style>
