<script setup>
import ProjectCard from "./mini-components/ProjectCard.vue";
import ProjectCardPopup from "./mini-components/ProjectCardPopup.vue";
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

// Active project
const activeProjectId = ref(null);

const openPopup = (id) => {
  activeProjectId.value = id;
};

const closePopup = () => {
  activeProjectId.value = null;
};

defineExpose({ addToList });

//Deleting a project
const deleteProject = (id) => {
  projectIds.value = projectIds.value.filter((pid) => pid !== id);
  localStorage.removeItem(`project-${id}`);
};
</script>

<template>
  <div
    class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-3 gap-12 md:gap-16 lg:gap-20 w-full max-w-[1600px] px-4 py-6 mx-auto"
  >
    <ProjectCard
      v-for="id in projectIds"
      :key="id"
      :projectId="id"
      @open="openPopup"
    />

    <ProjectCardPopup
      v-if="activeProjectId"
      :project-id="activeProjectId"
      @del="deleteProject"
      @close="closePopup"
    />
  </div>
</template>
