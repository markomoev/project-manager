<script setup>
import { ref, onMounted, computed, onBeforeUnmount } from "vue";
import ProjectCardPopup from "./ProjectCardPopup.vue";

//Getting the project Id
const props = defineProps({
  projectId: String,
});

// Defining project
const project = ref(null);

// Loading the project on localStorage
onMounted(() => {
  console.log("Loading project with ID:", props.projectId);
  const data = localStorage.getItem(`project-${props.projectId}`);
  console.log("Data from localStorage:", data);
  if (data) {
    project.value = JSON.parse(data);
  }
});

// Project status

const projectStatus = computed(() => {
  if (!project.value) return "";
  if (project.value.inProgress === true) {
    return "In Progress";
  } else {
    return "Completed";
  }
});

// Toggle Card Popup
const emit = defineEmits(["open"]);
const handleOpenPopup = () => {
  emit("open", props.projectId);
};

//Updating the project, when it is edited
onMounted(() => {
  loadProject();
});

function loadProject() {
  const data = localStorage.getItem(`project-${props.projectId}`);

  if (data) {
    project.value = JSON.parse(data);
  }
}

function handleUpdatedProject() {
  loadProject();
}
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
    ></textarea>

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
          class="block w-full bg-transparent border-green-400 text-green-200 text-base px-1 py-1 focus:outline-none transition placeholder:text-green-300 cursor-pointer sm:hover:border-green-300 break-all"
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

      <div class="open-btn">
        <button
          class="cursor-pointer share-btn mt-[7%] flex items-center justify-center rounded-xl bg-neutral-800 p-2 transition-all duration-200 sm:hover:bg-green-500/10 sm:hover:border-green-300 sm:hover:scale-105 sm:focus:outline-none"
          aria-label="Share project"
          @click="handleOpenPopup"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="#b9f8cf"
            width="1.7rem"
            height="1.7rem"
            viewBox="0 -32 576 576"
          >
            <path
              d="M527.9 224H480v-48c0-26.5-21.5-48-48-48H272l-64-64H48C21.5 64 0 85.5 0 112v288c0 26.5 21.5 48 48 48h400c16.5 0 31.9-8.5 40.7-22.6l79.9-128c20-31.9-3-73.4-40.7-73.4zM48 118c0-3.3 2.7-6 6-6h134.1l64 64H426c3.3 0 6 2.7 6 6v42H152c-16.8 0-32.4 8.8-41.1 23.2L48 351.4zm400 282H72l77.2-128H528z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>

  <ProjectCardPopup
    :project-id="project.id"
    v-if="cardPopupStatus"
    @del="() => emit('del', props.projectId)"
    @update="handleUpdatedProject"
  />
</template>

<style scoped>
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

/* 🔁 Responsive adjustments */
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

/* ✨ Hover and active effects only for non-phone screens */
@media (min-width: 641px) {
  .card-3d:hover {
    transform: perspective(800px) rotateY(8deg) rotateX(4deg) scale(1.04);
    box-shadow: 0 12px 32px #0005, 0 2px 8px #22c55e55;
    z-index: 10;
  }

  .card-3d:active {
    transform: perspective(800px) rotateY(-4deg) rotateX(-2deg) scale(0.98);
  }
}
</style>
