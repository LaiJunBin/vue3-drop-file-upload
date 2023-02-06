<script setup lang="ts">
import { ref } from "vue";

export interface Props {
  class?: string;
  enterClass?: string;
  multiple?: boolean;
  dropOnly?: boolean;
}

interface Emits {
  (e: "upload", files: FileList): void;
}

const isDragEnter = ref(false);
const fileInput = ref<HTMLInputElement | null>(null);
const props = withDefaults(defineProps<Props>(), {
  class: "",
  enterClass: "dfu-bg-blue-200",
  multiple: false,
  dropOnly: false,
});
const $emit = defineEmits<Emits>();

const onDragEnter = () => {
  isDragEnter.value = true;
};

const onDragLeave = () => {
  isDragEnter.value = false;
};

const onDrop = (e: DragEvent) => {
  onDragLeave();
  const files = e.dataTransfer?.files;
  if (files) {
    upload(files);
  }
};

const onChange = (e: Event) => {
  const files = (e.target as HTMLInputElement).files;
  if (files) {
    upload(files);
  }
};

const selectFile = () => {
  fileInput.value?.click();
};

const upload = (files: FileList) => {
  $emit("upload", files);
};

defineExpose({
  selectFile,
});
</script>

<template>
  <input
    type="file"
    ref="fileInput"
    class="dfu-hidden"
    :multiple="props.multiple"
    @change="onChange"
  />

  <div
    :class="['dfu-relative', props.class, { [props.enterClass]: isDragEnter }]"
  >
    <div
      class="dfu-absolute dfu-top-0 dfu-left-0 dfu-w-full dfu-h-full dfu-z-30"
      @dragenter.prevent="onDragEnter"
      @dragover.prevent=""
      @dragleave.prevent="onDragLeave"
      @drop.prevent="onDrop"
      @click="() => !props.dropOnly && selectFile()"
    ></div>
    <div>
      <slot>
        <div class="dfu-text-center">
          <slot name="icon"><div class="dfu-text-4xl">📁</div></slot>
          <div class="dfu-text-xl">
            <slot name="text">Drop file(s) here</slot>
          </div>
        </div>
      </slot>
    </div>
  </div>
</template>
