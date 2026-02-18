<template>
  <h1>{{ message }}</h1>
  <p v-if="status === 'active'">User is Active</p>
  <p v-else-if="status === 'pending'">User is Pending</p>
  <p v-else>User is Inactive</p>
  <h1>ha ha ha</h1>
  <h3>Tasks</h3>
  <ul>
    <li v-for="task in tasks" :key="task.id">
      <span
        :style="{ textDecoration: task.completed ? 'line-through' : 'none' }"
      >
        {{ task.text }}
      </span>
    </li>
  </ul>
  <!-- <a v-bind:href="link">{{ linkText }}</a> -->
  <a :href="link">{{ linkText }}</a>
  <br />
  <button v-on:click="toggleStatus">Toggle Status</button>
  <button @click="toggleStatus">Toggle Status</button>
</template>

<script>
export default {
  data() {
    return {
      message: "Hello Poss System!",
      status: "pending",
      tasks: [
        { id: 1, text: "Task 1", completed: false },
        { id: 2, text: "Task 2", completed: false },
        { id: 3, text: "Task 3", completed: true },
      ],
      link: "https://google.com",
      linkText: "Visit Google",
    };
  },

  methods: {
    toggleStatus() {
      if (this.status === "active") {
        this.status = "inactive";
      } else if (this.status === "pending") {
        this.status = "active";
      } else {
        this.status = "pending";
      }
    },
  },
};
</script>
