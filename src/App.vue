<script setup>
import Header from "./components/Header.vue";
import ProjectsContainer from "./components/ProjectsContainer.vue";
import { ref } from "vue";

const projectContainerRef = ref(null);
function handleProjectAdded() {
  projectContainerRef.value?.addToList();
}
</script>

<template>
  <div class="bg-neutral-900 min-h-screen text-white">
    <Header class="w-full h-auto" />
    <ProjectsContainer />
  </div>
</template>

<style scoped></style>
