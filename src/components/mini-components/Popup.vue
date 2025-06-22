<script setup>
import { ref, watch, onMounted } from "vue";

// Exporting the close emit to the Header
const emit = defineEmits(["close"]);

const closePopup = () => {
  emit("close");
};

const taskName = ref("");
const description = ref("");
const inProgress = ref(false);
const completed = ref(false);
const link = ref("");

// Creating the object for the projects
const project = ref({});
const projects = ref([]);

onMounted(() => {
  const saved = localStorage.getItem("projects");
  if (saved) {
    projects.value = JSON.parse(saved);
  }
});

const addProject = () => {
  project.value = {
    id: Date.now(),
    name: taskName,
    description: description,
    inProgress: inProgress,
    completed: completed,
    link: link,
  };
  console.log(project);
  emit("close");
};

// Saving the object in the localStorage
projects.value.push(project);
localStorage.setItem("project", JSON.stringify(projects.value));
</script>

<template>
  <!-- Overlay -->
  <div
    class="fixed inset-0 bg-opacity-50 flex items-center justify-center z-50"
  >
    <!-- Container -->
    <div
      class="bg-opacity-50 text-neutral-950 rounded-2xl shadow-xl w-full max-w-1/2 p-6 space-y-4"
    >
      <h2 class="text-xl font-semibold text-center text-green-50">
        Create a new Project
      </h2>

      <!-- Task Input -->
      <input
        type="text"
        v-model="taskName"
        placeholder="e.g. Task Manager"
        class="text-green-50 mr-[15%] bg-zinc-800 w-[40%] px-4 py-2 rounded-lg focus:outline-none"
      />

      <input
        type="text"
        v-model="description"
        placeholder="Write a desctiption"
        class="text-green-50 bg-zinc-800 w-[45%] px-4 py-2 rounded-lg focus:outline-none"
      />

      <div class="h-auto w-[40%] bg-zinc-800 rounded-lg p-[1%] text-green-50">
        <p class="text-base font-medium mb-[2%]">Choose a status:</p>
        <input
          type="checkbox"
          id="project-progress"
          v-model="inProgress"
          value="inProgress"
          class="checked:bg-amber-400 hover:cursor-pointer appearance-none w-3 h-3 border-2 rounded-full checked:rounded-full"
        />
        <label for="project-progress" class="ml-[2%]">In Progress</label>

        <br />

        <input
          type="checkbox"
          id="project-done"
          value="done"
          v-model="completed"
          class="checked:bg-green-400 hover:cursor-pointer appearance-none w-3 h-3 border-2 rounded-full checked:rounded-full"
        />
        <label for="project-done" class="ml-[2%]">Done</label>
      </div>

      <input
        type="url"
        id="project-link"
        v-model="link"
        placeholder="https://yourproject.com"
        class="h-auto w-[40%] bg-zinc-800 rounded-lg text-green-50 p-2"
      />
      <!-- Action Buttons -->
      <div class="flex justify-end space-x-2 pt-2">
        <button
          @click="closePopup"
          class="px-4 py-2 bg-zinc-800 rounded-lg cursor-pointer hover:bg-zinc-700 text-green-50 hover:text-green-300"
        >
          Cancel
        </button>
        <button
          @click="addProject"
          class="px-4 py-2 bg-green-300 text-zinc-800 rounded-lg cursor-pointer hover:bg-green-400"
        >
          Add
        </button>
      </div>
    </div>
  </div>
</template>
