<script setup>
import { ref, onMounted, computed } from "vue";

const emit = defineEmits([close]);

const props = defineProps({
  projectId: String,
});

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
</script>

<template>
  <div
    class="fixed inset-0 bg-black/60 backdrop-blur-sm flex items-center justify-center z-50 p-4"
  >
    <div
      class="bg-zinc-900 text-green-100 w-full max-w-3xl p-8 rounded-2xl shadow-2xl space-y-6 overflow-y-auto max-h-[90vh]"
    >
      <div v-if="project">
        <textarea
          v-model="project.name"
          readonly
          ref="nameArea"
          class="mb-[1%] w-full text-3xl font-bold bg-transparent border-b-1 border-green-400 focus:outline-none resize-none pb-[1%]"
          placeholder="Project name"
          rows="1"
          wrap="soft"
        />

        <textarea
          v-model="project.description"
          readonly
          class="justify-left w-full text-lg bg-transparent pt-[2%] rounded-md resize-none h-auto focus:outline-none"
          placeholder="Project description"
          rows="3"
        ></textarea>

        <div>
          <label class="block text-green-300 text-sm font-medium mb-[1%]"
            >Status:</label
          >
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
          <label class="block text-green-300 text-sm font-medium mb-[2%]"
            >Link:</label
          >
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
          <button
            @click="$emit('close')"
            class="mt-[5%] flex items-center justify-center rounded-xl cursor-pointer bg-neutral-800 pr-[2%] pl-[2%] pt-[1%] pb-[1%] transition-all duration-200 hover:bg-green-500/10 hover:border-green-300 hover:scale-105 focus:outline-none"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
