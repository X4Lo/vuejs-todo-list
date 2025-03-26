<script setup>
import { ref } from 'vue';
// import HelloWorld from './components/HelloWorld.vue'
const title = ref('')
const tasks = ref([
  {
    id: 1,
    title: "Eat food",
    status: "DONE"
  }
]);

function handleAdd() {
  if (title.value.trim() == '') return;

  console.log(tasks.value)
  tasks.value.push({
    id: tasks.value[tasks.value.length - 1].id + 1,
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
  <div class="">
    <div>
      <div class="flex">
        <label for="task-input">Add Task</label>
        <input type="text" id="task-input" v-model="title" placeholder="Insert task title" />
      </div>
      <button v-on:click="handleAdd()">Add</button>
    </div>

    <div>
      <ul>
        <div v-for="(task, i) in tasks" :key="i">
          <span>{{ task.title }}</span>
          <button v-on:click="onChangeStatus(task.id)">{{ task.status }}</button>
        </div>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
