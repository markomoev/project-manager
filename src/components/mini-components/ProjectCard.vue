<script setup>
import { ref, onMounted } from "vue";

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
</script>

<template>
  <div class="card-3d">
    <input
      v-if="project"
      v-model="project.name"
      readonly
      class="text-lg font-semibold mb-2 bg-transparent border-b-1 border-green-400 focus:outline-none focus:border-green-300 text-green-100 w-full px-1 py-1 transition"
      :placeholder="'Project name'"
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
      <button
        class="absolute bottom-2 right-2 bg-green-500 hover:bg-green-400 text-green-900 font-semibold py-1 px-3 rounded-lg transition shadow"
      >
        ...
      </button>
    </div>
  </div>
</template>

<style scoped>
.card-3d {
  background: #262626;
  width: 16rem;
  padding: 1rem;
  border-radius: 0.75rem;
  box-shadow: 0 4px 24px #0002;
  margin: 1.5rem;
  transition: transform 0.3s, box-shadow 0.3s;
  will-change: transform;
  perspective: 800px;
  position: relative;
}
.card-3d:hover {
  transform: perspective(800px) rotateY(8deg) rotateX(4deg) scale(1.04);
  box-shadow: 0 12px 32px #0005, 0 2px 8px #22c55e55;
  z-index: 10;
}
.card-3d:active {
  transform: perspective(800px) rotateY(-4deg) rotateX(-2deg) scale(0.98);
}
</style>
