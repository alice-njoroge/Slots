<script setup>
const props = defineProps({
  modelValue: {type: String, default: '', required: 'true'},
  type: {type: String, default: 'input', required: true},
  placeholder: {type: String, default: ''},
  disabled: {type: Boolean, default: false},
  name: {type: String, default: ''}
});
const emit = defineEmits({
  'update:modelValue': null
});

const onInput = (event) => {
  emit('update:modelValue', event.target.value)
}
//notice that I used v-bind attrs without importing useAttrs() this time!!
</script>

<template>
  <div class="movie-form-input-wrapper">
    <label :for="name">
      <slot name="label">{{ name }}</slot>
    </label>
    <input
        :type="type"
        :disabled="disabled"
        :placeholder="placeholder"
        :name="name"
        :value="modelValue"
        @input="onInput"
        class="movie-form-input"
        v-bind="$attrs"
    />
    <span class="movie-form-error"><slot name="err-span">{{name}} is required</slot></span>
  </div>

</template>

<style scoped>

</style>