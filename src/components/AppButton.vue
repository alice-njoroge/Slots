<script setup>
const props = defineProps({
  type: {type: String, default: 'button', required: true},
  disabled: {type: Boolean, default: false},

})

</script>

<template>
  <button
      class="self-end movie-actions-list-action-button button-primary justify-self-end"
      @click="$emit('click')"
  >
    <slot></slot>
  </button>
</template>

<style scoped>


</style>