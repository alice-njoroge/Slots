<script setup>
import {XMarkIcon} from "@heroicons/vue/20/solid";

defineProps({
  title: {type: String, default: 'Modal Title'}
});
</script>

<template>
  <div class="modal-wrapper">

    <div class="modal-wrapper-inner">
      <div class="flex text-right">
        <button class="h-5 w-4 top-0 right-0 inline-block"  @click="$emit('close')"> <XMarkIcon /> </button>
      </div>
      <slot name="title">{{ title }}</slot>
      <slot name="body"></slot>
    </div>
  </div>
</template>
