<script setup>
import { ref, onMounted, computed } from "vue";

// Получаваме пропса с ID-то на проекта
const props = defineProps({
  projectId: String,
});

// Дефинираме референция за проекта
const project = ref(null);

// Зареждаме проекта от localStorage
onMounted(() => {
  console.log("Loading project with ID:", props.projectId);
  const data = localStorage.getItem(`project-${props.projectId}`);
  console.log("Data from localStorage:", data);
  if (data) {
    project.value = JSON.parse(data);
  }
});

// Правим статута на задачата

const projectStatus = computed(() => {
  if (!project.value) return "";
  if (project.value.inProgress === true) {
    return "In Progress";
  } else {
    return "Completed";
  }
});
</script>

<template>
  <div class="card-3d">
    <textarea
      v-if="project"
      v-model="project.name"
      readonly
      ref="nameArea"
      class="overflow-hidden text-lg font-semibold mb-2 bg-transparent border-b-1 border-green-400 focus:outline-none focus:border-green-300 text-green-100 w-full px-1 py-1 transition resize-none"
      :placeholder="'Project name'"
      rows="1"
      wrap="soft"
    />

    <div class="relative">
      <textarea
        v-if="project"
        v-model="project.description"
        readonly
        class="h-auto overflow-hidden w-full bg-transparent text-green-100 px-2 py-1 mt-2 resize-none focus:outline-none focus:border-green-300 transition placeholder:text-green-300"
        :placeholder="'Project description'"
        rows="3"
      ></textarea>
    </div>

    <div class="status mt-4">
      <label class="block text-green-300 text-sm font-medium mb-1"
        >Status:</label
      >
      <div class="flex items-center gap-2">
        <span
          :class="[
            'inline-block w-3 h-3 rounded-full',
            projectStatus === 'In Progress' ? 'bg-amber-400' : 'bg-green-400',
          ]"
          aria-label="status dot"
        ></span>
        <input
          v-if="project"
          :value="projectStatus"
          readonly
          tabindex="-1"
          class="flex-1 bg-transparent border-green-400 text-green-200 text-base font-semibold px-1 py-1 focus:outline-none transition placeholder:text-green-300 cursor-default"
        />
      </div>

      <div class="link mt-10">
        <label class="block text-green-300 text-sm font-medium mb-1"
          >Link:</label
        >
        <a
          v-if="project && project.link"
          :href="project.link"
          target="_blank"
          rel="noopener noreferrer"
          class="block w-full bg-transparent border-green-400 text-green-200 text-base px-1 py-1 focus:outline-none transition placeholder:text-green-300 cursor-pointer hover:border-green-300 break-all"
        >
          {{ project.link }}
        </a>
        <span
          v-else
          class="block w-full bg-transparent border-b-2 border-green-400 text-green-400 text-base px-1 py-1 opacity-50"
        >
          https://yourproject.com
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* In <style scoped> of ProjectCard.vue */
.card-3d {
  background: #262626;
  width: 100%;
  max-width: 370px;
  min-width: 0;
  padding: 1.5rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 24px #0002;
  margin: 0 auto;
  margin-top: 2rem;
  transition: transform 0.3s, box-shadow 0.3s;
  will-change: transform;
  perspective: 800px;
  position: relative;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  height: 100%;
}
@media (max-width: 1024px) {
  .card-3d {
    max-width: 48vw;
    padding: 1.2rem;
  }
}
@media (max-width: 640px) {
  .card-3d {
    max-width: 98vw;
    padding: 1rem;
    margin-top: 1rem;
  }
}
.card-3d:hover {
  transform: perspective(800px) rotateY(8deg) rotateX(4deg) scale(1.04);
  box-shadow: 0 12px 32px #0005, 0 2px 8px #22c55e55;
  z-index: 10;
}
.card-3d:active {
  transform: perspective(800px) rotateY(-4deg) rotateX(-2deg) scale(0.98);
}

@media (max-width: 1024px) {
  .card-3d {
    max-width: 48vw;
    padding: 1.2rem;
  }
}
@media (max-width: 640px) {
  .card-3d {
    max-width: 98vw;
    padding: 1rem;
    margin-top: 1rem;
  }
}
</style>
