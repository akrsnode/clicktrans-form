<script setup>
defineProps({
  label: String,
  maxLength: Number,
  modelValue: String,
  errorMsg: String,
});
defineEmits(["update:modelValue"]);
</script>

<template>
  <div
    class="a-formInput"
    :class="{ '-invalid': errorMsg || modelValue.length > maxLength }"
  >
    <label :for="label" class="a-formInput__label">{{ label }}</label>
    <textarea
      placeholder=""
      :id="label"
      class="a-formInput__textarea"
      rows="5"
      required
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    ></textarea>
    <div class="a-formInput__helpers">
      <span class="a-formInput__error" v-if="errorMsg">{{ errorMsg }}</span>
      <span :class="{ 'a-formInput__error': modelValue.length > maxLength }"
        >{{ modelValue.length }}/{{ maxLength }}</span
      >
    </div>
  </div>
</template>
