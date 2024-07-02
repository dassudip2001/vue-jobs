<script setup>
import { ref,onMounted  } from "vue";
const name = ref("sudip");
const status = ref("active");
const task = ref(["Task one", "Task two"]);
const newTask = ref("");

const toggleStats = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if (newTask.value.trim() === "") {
    return;
  }
  task.value.push(newTask.value);
  newTask.value = "";
};

const remove = (index) => {
  task.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response= await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    task.value = data.map((d) => d.title);
  } catch (error) {
    console.log(error);
  }
}),
</script>

<template>
  <h1>{{ name }}</h1>
  <p>Status: {{ status }}</p>

  <form @submit.prevent="addTask">
    <label for="newTask">New Task</label>
    <input type="text" v-model="newTask" name="newTask" />
    <button type="submit">Add Task</button>
  </form>

  <ul>
    <li v-for="(t, index) in task" :key="index">
      {{ t }}
      <p @click="remove(index)">Remove</p>
    </li>
  </ul>
  <button @click="toggleStats">Toggle Status</button>
</template>

<style></style>
