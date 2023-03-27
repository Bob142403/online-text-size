<script setup>
import { computed, ref, watch } from "vue";
import NavBar from "./components/NavBar.vue";

const input = ref("");
const txtArea = ref(null);

const size = computed(() => new Blob([input.value]).size);

watch(input, () => {
  autoGrow();
});

function res(event) {
  const file = event.target.files[0];

  const reader = new FileReader();

  reader.readAsText(file);

  reader.onload = function () {
    txtArea.value.value = reader.result;
    input.value = reader.result;
    autoGrow();
  };

  reader.onerror = function () {
    input.value = reader.error;
  };
}

function autoGrow() {
  if (txtArea.value.scrollHeight > txtArea.value.clientHeight) {
    txtArea.value.style.height = `${txtArea.value.scrollHeight}px`;
    // while (
    //   parseInt(event.target.style.height) >= event.target.scrollHeight &&
    //   event.target.scrollHeight > 140
    // ) {
    //   console.log(event.target.style.height);
    //   event.target.style.height = `${
    //     parseInt(event.target.style.height) - 20
    //   }px`;
    // }
  }
}
</script>

<template>
  <NavBar></NavBar>
  <div class="container mx-auto pt-3 flex flex-col">
    <input
      class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 dark:text-gray-400 focus:outline-none dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400"
      type="file"
      @change="res"
    />
    <textarea
      rows="5"
      class="overflow-hidden block p-2.5 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 mt-3"
      placeholder="Введите что нибудь...."
      v-model="input"
      ref="txtArea"
    ></textarea>
    <span class="" v-if="size >= 1024"
      >Размер в Килобайтах: {{ size / 1024 }}</span
    >
    <span v-else class="">Размер в Байтах: {{ size }}</span>
  </div>
</template>
