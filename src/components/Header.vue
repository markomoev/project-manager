<script setup>
import { ref, onMounted, watch } from "vue";
import Popup from "./mini-components/Popup.vue";

// Setting a username and saving it to localStorage
const username = ref("");
watch(username, (val) => {
  localStorage.setItem("username", val);
});
onMounted(() => {
  const saved = localStorage.getItem("username");
  if (saved) {
    username.value = saved;
  }
});

// Show popup
const popupStatus = ref();

const togglePopup = () => {
  popupStatus.value = ref(true);
};
</script>

<template>
  <header
    class="flex flex-col sm:flex-row items-center sm:justify-between gap-4 sm:gap-0 px-4 sm:px-6 py-4 bg-neutral-900 border-b border-zinc-800 shadow"
  >
    <!-- Heading + Input container -->
    <div
      class="w-full sm:w-auto flex flex-col sm:flex-row items-center sm:items-start justify-center sm:justify-start gap-2 sm:gap-3 text-center sm:text-left"
      :class="{
        'ml-0': true,
        'sm:ml-[15%]': true,
      }"
    >
      <h1 class="text-xl sm:text-2xl font-bold text-emerald-200">
        Welcome<span class="hidden sm:inline">,</span>
      </h1>
      <!-- Input shown separately on mobile, inline on desktop -->
      <input
        v-model="username"
        class="bg-transparent text-emerald-100 font-semibold px-2 py-1 mt-1 sm:mt-0 sm:ml-2 focus:outline-none placeholder:text-emerald-300 text-center text-lg sm:text-base sm:text-left"
        placeholder="Enter your name"
      />
    </div>

    <!-- Button container -->
    <div
      class="w-full sm:w-auto sm:mr-[15%] flex justify-center sm:justify-end"
    >
      <button
        class="w-full sm:w-auto bg-green-200 cursor-pointer hover:bg-green-300 text-green-900 text-base font-medium rounded-lg px-5 py-2 transition-shadow border border-green-300 shadow-none"
        @click="togglePopup"
        title="Add Project"
      >
        Add Project
      </button>
    </div>

    <Popup v-if="popupStatus" @close="popupStatus = false" />
  </header>
</template>
