<script setup>
import JobListing from "./JobListing.vue";
import { reactive, defineProps, onMounted } from "vue";
import { RouterLink } from "vue-router";
import axios from "axios";
import { PulseLoader  } from "vue-spinner";

defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: false,
  },
});

const state = reactive({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get("/api/jobs");
    state.jobs = response.data;
  } catch (error) {
    console.error("Error fetching jobs:", error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <!-- Browse Jobs -->
  <section class="bg-green-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
        Browse Jobs
      </h2>
      
      <div v-if="state.isLoading" class="flex justify-center items-center my-10">
        <PulseLoader :loading="state.isLoading" color="#16a34a" size="15px" />
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <!-- Job Listing 1 -->
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>
    </div>
  </section>

  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <RouterLink
      to="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</RouterLink
    >
  </section>
</template>
