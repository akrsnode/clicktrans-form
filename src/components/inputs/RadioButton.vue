<script setup>
defineProps({
  label: String,
  modelValue: Boolean,
  errorMsg: String,
});
defineEmits(["update:modelValue"]);
</script>

<template>
  <div class="a-formInput" :class="{ '-invalid': errorMsg }">
    <span class="a-formInput__label">{{ label }}</span>

    <div class="a-formInput__option">
      <input
        id="yes"
        type="radio"
        name="sendConfirmation"
        :value="true"
        @click="$emit('update:modelValue', true)"
      />
      <label for="yes" class="a-formInput__optionLabel">Yes</label>
    </div>

    <div class="a-formInput__option">
      <input
        id="no"
        type="radio"
        name="sendConfirmation"
        :value="false"
        @click="$emit('update:modelValue', false)"
      />
      <label for="no" class="a-formInput__optionLabel">No</label>
    </div>

    <div class="a-formInput__helpers">
      <span class="a-formInput__error" v-if="errorMsg">{{ errorMsg }}</span>
    </div>
  </div>
</template>
