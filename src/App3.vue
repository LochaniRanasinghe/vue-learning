<template>
  <h1>{{ message }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>
  <button v-on:click="toggleStatus">Toggle Status</button>
  <!-- <button @click="toggleStatus">Toggle Status</button> -->

  <br /><br />
  <a :href="link">{{ linkText }}</a>
  <br /><br /><br />

  <form @submit.prevent="addTask">
    <label for="newTask">Enter New Task:</label>
    <input
      id="newTask"
      name="newTask"
      v-model="newTask"
      placeholder="New Task"
    />
    <br />
    <button type="submit">Add Task</button>
  </form>

  <br /><br />
  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task.id">
      <span
        :style="{ textDecoration: task.completed ? 'line-through' : 'none' }"
      >
        {{ task.text }}
      </span>
      <button @click="deleteTask(task.id)">X</button>
    </li>
  </ul>

  <br />
</template>

<script setup>
import { ref, onMounted } from "vue";

//reactive variables
const message = ref("Hello Poss System!");
const status = ref("pending");
const tasks = ref([
  { id: 1, text: "Task 1", completed: false },
  { id: 2, text: "Task 2", completed: false },
  { id: 3, text: "Task 3", completed: true },
]);
const newTask = ref("FUDGE");

const link = ref("https://google.com");
const linkText = ref("Visit Google");

function toggleStatus() {
  if (status.value === "active") {
    status.value = "inactive";
  } else if (status.value === "pending") {
    status.value = "active";
  } else {
    status.value = "pending";
  }
}

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      id: tasks.value.length + 1,
      text: newTask.value,
      completed: false,
    });
    newTask.value = "";
  }
};

const deleteTask = (taskId) => {
  tasks.value.splice(
    tasks.value.findIndex((task) => task.id === taskId),
    1,
  );
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.slice(0, 5).map((item) => ({
      id: item.id,
      text: item.title,
      completed: item.completed,
    }));
  } catch (error) {
    console.error("Error fetching tasks:", error);
  }
});
</script>
