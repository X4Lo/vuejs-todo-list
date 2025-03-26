<script setup>
import { ref } from 'vue';
const title = ref('')
const tasks = ref([]);

function handleAdd() {
  if (title.value.trim() == '') return;

  let newId = 1;
  if (tasks.value.length != 0)
    newId = tasks.value[tasks.value.length - 1].id + 1;

  tasks.value.push({
    id: newId,
    title: title.value.trim(),
    status: "TODO"
  });

  title.value = "";
}

function onChangeStatus(id) {
  for (let i = 0; i < tasks.value.length; i++) {
    if (tasks.value[i].id == id) {
      tasks.value[i].status = cycleStatus(tasks.value[i].status);
      break;
    }
  }
}

function cycleStatus(status) {
  switch (status) {
    case "TODO":
      return "DOING"
    case "DOING":
      return "DONE"
    case "DONE":
      return "TODO"
    default:
      return "TODO"
  }
}

</script>

<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100">
    <div class="w-full max-w-2xl p-6 bg-white rounded-lg shadow-md">
      <div class="flex items-center gap-4 mb-6">
        <label for="task-input" class="text-gray-700 font-medium">Add Task</label>
        <input type="text" id="task-input" v-model="title" placeholder="Insert task title"
          class="flex-grow px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" />
        <button @click="handleAdd"
          class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors">
          Add
        </button>
      </div>

      <ul class="space-y-3">
        <li v-for="(task, i) in tasks" :key="i"
          class="flex justify-between items-center px-4 py-3 bg-gray-50 rounded-md shadow-sm">
          <span class="text-gray-800">{{ task.title }}</span>
          <button @click="onChangeStatus(task.id)" :class="[
            task.status === 'TODO'
              ? 'bg-yellow-500 hover:bg-yellow-600'
              : task.status === 'DOING'
                ? 'bg-blue-500 hover:bg-blue-600'
                : 'bg-green-500 hover:bg-green-600',
            'px-3 py-1 text-white rounded-md transition-colors'
          ]">
            {{ task.status }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
</style>
