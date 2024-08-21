<script setup>
import {useAttrs} from "vue";

const props = defineProps({
  type: {type: String, default: 'button', required: true},
  disabled: {type: Boolean, default: false},
});
const $attrs = useAttrs();

</script>

<template>
  <button
      class="self-end movie-actions-list-action-button button-primary justify-self-end"
      @click="$emit('click')"
      v-bind="$attrs"
  >
    <slot></slot>
  </button>
</template>

<style scoped>


</style>