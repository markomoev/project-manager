<script setup>
import { ref } from "vue";

// Exporting the close emit to the Header
const emit = defineEmits(["close", "project-added"]);

const closePopup = () => {
  emit("close");
};

const taskName = ref("");
const description = ref("");
const inProgress = ref(false);
const completed = ref(false);
const link = ref("");

// Creating the object for every project
const project = ref({});

const addProject = async () => {
  try {
    project.value = {
      id: Date.now(),
      name: taskName.value,
      description: description.value,
      inProgress: inProgress.value,
      completed: completed.value,
      link: link.value,
    };

    localStorage.setItem(
      `project-${project.value.id}`,
      JSON.stringify(project.value)
    );

    console.log(project.value);
    emit("close");
    emit("project-added");
    window.dispatchEvent(new Event("project-added"));
  } catch (error) {
    console.log("Oops! Something went wrong! Error details:" + error);
  }
};

// Saving the object in the localStorage
</script>

<template>
  <!-- Overlay with blur -->
  <div
    class="fixed inset-0 bg-black/30 backdrop-blur-sm flex items-center justify-center z-50"
  >
    <!-- Container -->
    <div
      class="bg-neutral-900 text-green-50 rounded-2xl shadow-2xl w-[92vw] max-w-lg p-6 space-y-5 border border-zinc-800"
    >
      <h2 class="text-xl font-semibold text-center text-green-100 mb-2">
        Create a new Project
      </h2>

      <!-- Task Input -->
      <input
        type="text"
        v-model="taskName"
        placeholder="e.g. Task Manager"
        :class="[
          'w-full bg-zinc-800 text-green-50 px-4 py-2 rounded-lg border-2 focus:outline-none focus:ring-2 placeholder:text-green-200 transition',
          taskName.length === 25
            ? 'border-red-500 focus:ring-red-400'
            : 'border-green-400 focus:ring-green-400',
        ]"
        @input="if (taskName.length > 25) taskName = taskName.slice(0, 25);"
      />

      <input
        type="text"
        v-model="description"
        placeholder="Write a description"
        :class="[
          'w-full bg-zinc-800 text-green-50 px-4 py-2 rounded-lg border-2 focus:outline-none focus:ring-2 placeholder:text-green-200 transition',
          description.length === 150
            ? 'border-red-500 focus:ring-red-400'
            : 'border-green-400 focus:ring-green-400',
        ]"
        @input="
          if (description.length > 150) description = description.slice(0, 150);
        "
      />

      <div class="bg-zinc-800 rounded-lg p-3 text-green-50">
        <p class="text-base font-medium mb-2">Choose a status:</p>
        <label class="inline-flex items-center mr-4">
          <input
            type="checkbox"
            v-model="inProgress"
            class="accent-amber-400 w-4 h-4 rounded focus:ring-2 focus:ring-amber-400"
          />
          <span class="ml-2">In Progress</span>
        </label>
        <label class="inline-flex items-center">
          <input
            type="checkbox"
            v-model="completed"
            class="accent-green-400 w-4 h-4 rounded focus:ring-2 focus:ring-green-400"
          />
          <span class="ml-2">Done</span>
        </label>
      </div>

      <input
        type="url"
        v-model="link"
        placeholder="https://yourproject.com"
        class="w-full bg-zinc-800 rounded-lg text-green-50 px-4 py-2 focus:outline-none focus:ring-2 focus:ring-green-400 placeholder:text-green-200"
      />

      <!-- Action Buttons -->
      <div class="flex justify-end gap-3 pt-2">
        <button
          @click="closePopup"
          class="px-4 py-2 bg-green-200 rounded-lg cursor-pointer hover:bg-green-300 text-green-900 hover:text-green-800 transition"
        >
          Cancel
        </button>
        <button
          @click="addProject"
          class="px-4 py-2 bg-green-400 text-green-900 rounded-lg cursor-pointer hover:bg-green-500 transition"
        >
          Add
        </button>
      </div>
    </div>
  </div>
</template>
