<script setup>
defineProps({
  label: String,
  modelValue: [String, Number],
  disabled: Boolean,
  disabledTooltip: String,
  errorMsg: String,
});
defineEmits(["update:modelValue"]);
</script>

<template>
  <div
    class="a-formInput"
    :class="{ '-invalid': errorMsg }"
    :data-tip="disabledTooltip"
  >
    <label :for="label" class="a-formInput__label">{{ label }}</label>
    <input
      :id="label"
      class="a-formInput__textInput"
      type="text"
      :value="modelValue"
      :disabled="disabled"
      required
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <div class="a-formInput__helpers">
      <span class="a-formInput__error" v-if="errorMsg">{{ errorMsg }}</span>
    </div>
  </div>
</template>
