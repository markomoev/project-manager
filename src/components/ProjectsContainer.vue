<script setup>
import ProjectCard from "./mini-components/ProjectCard.vue";
import { ref, onMounted, onBeforeUnmount } from "vue";

const projectIds = ref([]);
const emit = defineEmits(["addToList"]);
// we add the component
const addToList = () => {
  projectIds.value = Object.keys(localStorage)
    .filter((key) => key.startsWith("project-"))
    .sort((a, b) => {
      // sort them by when is the card added

      const aTime = Number(a.replace("project-", ""));
      const bTime = Number(b.replace("project-", ""));
      return aTime - bTime;
    })
    .map((key) => key.replace("project-", ""));
};

onMounted(() => {
  addToList();
  window.addEventListener("project-added", addToList);
});

onBeforeUnmount(() => {
  window.removeEventListener("project-added", addToList);
});

defineExpose({ addToList });
</script>

<template>
  <div class="flex flex-wrap gap-4 w-full">
    <ProjectCard v-for="id in projectIds" :key="id" :projectId="id" />
  </div>
</template>
