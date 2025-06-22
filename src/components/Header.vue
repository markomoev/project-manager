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

function togglePopup() {
  popupStatus.value = ref(true);
}
</script>

<template>
  <div
    class="inline-flex items-center gap-0 text-emerald-50 border-b border-solid border-zinc-800"
  >
    <h1 class="ml-[1%] m-[0.8%] text-2xl font-bold">
      Welcome,
      <input
        v-model="username"
        class="bg-transparent border-none focus:outline-none focus:ring-0"
        placeholder="Enter your name"
      />
    </h1>

    <button
      class="ml-[50%] p-[0.30%] pl-[0.7%] pr-[0.7%] flex items-center justify-center bg-zinc-800 cursor-pointer text-emerald-50 text-xl font-mono rounded-xl hover:bg-zinc-700 hover:text-green-300"
      @click="togglePopup"
    >
      +
    </button>

    <Popup v-if="popupStatus" @close="popupStatus = false" />
  </div>
</template>
