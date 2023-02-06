<script setup lang="ts">
import { ref } from "vue";
import TheDropFileUpload from "./components/TheDropFileUpload.vue";

const dropFileUpload = ref<InstanceType<typeof TheDropFileUpload> | null>(null);
const onUpload = (files: FileList) => {
  console.log(files);
};
</script>

<template>
  <h1>Demo Drop File Upload</h1>
  <h3>
    Please open console tab(F12) then select the file or drag & drop the file.
  </h3>
  <TheDropFileUpload
    @upload="onUpload"
    class="dfu-w-96 dfu-h-96 dfu-border dfu-border-solid dfu-border-gray-200 dfu-mt-4 dfu-flex dfu-items-center dfu-justify-center"
    ref="dropFileUpload"
  >
    <template #text>
      <div>Drop file(s) here</div>
      <small class="dfu-text-sm">OR</small>
      <div
        class="dfu-relative dfu-mt-1 dfu-border dfu-border-solid dfu-border-gray-500 dfu-rounded-xl dfu-z-50 dfu-cursor-pointer dfu-select-none"
        @click="dropFileUpload?.selectFile"
      >
        Select file(s)
      </div>
    </template>
  </TheDropFileUpload>
</template>
