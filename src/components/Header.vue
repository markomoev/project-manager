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
    class="flex items-center justify-between px-6 py-4 bg-neutral-900 border-b border-zinc-800 shadow"
  >
    <div class="flex items-center gap-3 ml-[15%]">
      <h1 class="text-2xl font-bold text-emerald-200">
        Welcome,
        <input
          v-model="username"
          class="bg-transparent text-emerald-100 font-semibold px-2 py-1 ml-2 focus:outline-none placeholder:text-emerald-300"
          placeholder="Enter your name"
        />
      </h1>
    </div>
    <button
      class="bg-green-200 mr-[15%] cursor-pointer hover:bg-green-300 text-green-900 text-base font-medium rounded-lg px-5 py-2 transition-shadow border border-green-300 shadow-none"
      @click="togglePopup"
      title="Add Project"
    >
      Add Project
    </button>
    <Popup v-if="popupStatus" @close="popupStatus = false" />
  </header>
</template>
