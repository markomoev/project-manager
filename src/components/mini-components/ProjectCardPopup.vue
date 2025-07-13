<script setup>
import { ref, onMounted, computed, watch } from "vue";

// Defining emits and props
const emit = defineEmits(["del", "update"]);

const props = defineProps({
  projectId: String,
});

// Object for every project
const project = ref(null);

// Loading the project from localStorage
onMounted(() => {
  const data = localStorage.getItem(`project-${props.projectId}`);
  if (data) {
    project.value = JSON.parse(data);
  }
});

// Project Status
const projectStatus = computed(() => {
  if (!project.value) return "";
  if (project.value.inProgress === true) {
    return "In Progress";
  } else {
    return "Completed";
  }
});

// Button for deleting the project
const closeAndDel = () => {
  emit("close");
  emit("del", props.projectId);
};

// Editing functionality
const isEditing = ref(false);
const editedProject = ref({}); // We spread the object to get its elements

watch(
  project,
  (newVal) => {
    if (newVal) {
      editedProject.value = { ...newVal };
    }
  },
  { immediate: true }
);

function startEditing() {
  isEditing.value = true;
  editedProject.value = { ...project.value };
}

function stopEditing() {
  isEditing.value = false;
}

function saveEdit() {
  if (!editedProject.value || !editedProject.value.id) return;

  // Save updated project to localStorage
  localStorage.setItem(
    `project-${editedProject.value.id}`,
    JSON.stringify(editedProject.value)
  );

  project.value = { ...editedProject.value };

  emit("update", editedProject.value);
  isEditing.value = false;
  location.reload();
}
</script>

<template>
  <div
    class="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center z-50 p-4"
  >
    <div
      class="relative bg-zinc-900 text-green-100 w-full max-w-3xl p-8 rounded-2xl shadow-2xl space-y-6 overflow-y-auto max-h-[90vh]"
    >
      <div v-if="project">
        <textarea
          v-model="editedProject.name"
          :readonly="!isEditing"
          ref="nameArea"
          class="mb-[1%] w-full text-3xl font-bold bg-transparent border-b-1 border-green-400 focus:outline-none resize-none pb-[1%]"
          placeholder="Project name"
          rows="1"
          wrap="soft"
        ></textarea>

        <textarea
          v-model="editedProject.description"
          :readonly="!isEditing"
          class="justify-left w-full text-lg bg-transparent pt-[2%] rounded-md resize-none h-auto focus:outline-none"
          placeholder="Project description"
          rows="3"
        ></textarea>

        <div>
          <label class="block text-green-300 text-sm font-medium mb-[1%]">
            Status:
          </label>
          <div class="flex items-center gap-2">
            <span
              :class="[
                'inline-block w-4 h-4 rounded-full mb-[3%]',
                projectStatus === 'In Progress'
                  ? 'bg-amber-400'
                  : 'bg-green-400',
              ]"
              aria-label="status dot"
            ></span>
            <input
              :value="projectStatus"
              readonly
              tabindex="-1"
              class="flex-1 bg-transparent text-green-200 text-base font-semibold focus:outline-none cursor-default mb-[3%]"
            />
          </div>
        </div>

        <div>
          <label class="block text-green-300 text-sm font-medium mb-[2%]">
            Link:
          </label>
          <a
            v-if="project.link"
            :href="project.link"
            target="_blank"
            rel="noopener noreferrer"
            class="block text-green-300 underline break-all hover:text-green-100 transition"
          >
            {{ project.link }}
          </a>
          <span
            v-else
            class="block text-green-500 opacity-50 border-b border-dashed border-green-400"
          >
            https://yourproject.com
          </span>
        </div>

        <div>
          <!--Close button-->
          <button
            @click="emit('close')"
            class="cursor-pointer absolute top-4 right-4 text-green-200 hover:text-green-100 transition focus:outline-none"
            aria-label="Close"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-6 h-6"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>

        <button
          @click="closeAndDel"
          aria-label="Delete project"
          class="cursor-pointer absolute bottom-7.5 right-6 bg-red-900/30 text-red-200 rounded-lg px-4 py-2 border border-red-700 hover:bg-red-500/10 hover:border-red-400 hover:scale-105 transition-all duration-200 focus:outline-none shadow-md"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
          >
            <path
              d="M3.03919 4.2939C3.01449 4.10866 3.0791 3.92338 3.23133 3.81499C3.9272 3.31953 6.3142 2 12 2C17.6858 2 20.0728 3.31952 20.7687 3.81499C20.9209 3.92338 20.9855 4.10866 20.9608 4.2939L19.2616 17.0378C19.0968 18.2744 18.3644 19.3632 17.2813 19.9821L16.9614 20.1649C13.8871 21.9217 10.1129 21.9217 7.03861 20.1649L6.71873 19.9821C5.6356 19.3632 4.90325 18.2744 4.73838 17.0378L3.03919 4.2939Z"
              stroke="#fca5a5"
              stroke-width="1.5"
            />
            <path
              d="M3 5C5.57143 7.66666 18.4286 7.66662 21 5"
              stroke="#fca5a5"
              stroke-width="1.5"
            />
            <path
              d="M11 18L14 14.5M14 14.5L19 17M14 14.5L20 11.5"
              stroke="#fca5a5"
              stroke-width="1.5"
            />
            <path
              d="M4.5 16L7.73595 15.5377C7.90405 15.5137 8.07446 15.562 8.20491 15.6708L11 18L14 21"
              stroke="#fca5a5"
              stroke-width="1.5"
            />
            <path
              d="M8 15.5L10.6149 12.4493C10.8284 12.2001 11.2025 12.1688 11.4546 12.3788L14 14.5"
              stroke="#fca5a5"
              stroke-width="1.5"
            />
          </svg>
        </button>

        <div class="flex flex-wrap gap-4 mt-6">
          <button
            @click="startEditing"
            class="cursor-pointer px-4 py-2 bg-green-900/30 text-green-100 font-semibold rounded-xl border border-green-700 hover:bg-green-500/10 hover:border-green-400 hover:scale-105 transition-all duration-200 focus:outline-none"
          >
            Edit
          </button>

          <button
            v-if="isEditing"
            @click="stopEditing"
            class="cursor-pointer px-4 py-2 bg-yellow-900/20 text-yellow-200 font-semibold rounded-xl border border-yellow-700 hover:bg-yellow-500/10 hover:border-yellow-400 hover:scale-105 transition-all duration-200 focus:outline-none"
          >
            Stop Editing
          </button>

          <button
            v-if="isEditing"
            @click="saveEdit"
            class="cursor-pointer px-4 py-2 bg-emerald-900/30 text-emerald-100 font-semibold rounded-xl border border-emerald-700 hover:bg-emerald-500/10 hover:border-emerald-400 hover:scale-105 transition-all duration-200 focus:outline-none"
          >
            Save Changes
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
