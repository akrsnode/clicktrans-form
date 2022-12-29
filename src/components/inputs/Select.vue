<script setup>
defineProps({
  label: String,
  placeholder: String,
  options: Array,
  errorMsg: String,
});
defineEmits(["update:modelValue"]);
</script>

<template>
  <div class="a-formInput" :class="{ '-invalid': errorMsg }">
    <label for="label" class="a-formInput__label">{{ label }}</label>
    <select
      class="a-formInput__select"
      :id="label"
      :name="label"
      @change="$emit('update:modelValue', $event.target.value)"
      required
    >
      <option v-if="placeholder" value="" disabled selected hidden>
        {{ placeholder }}
      </option>
      <option
        v-for="option in options"
        :value="option.value"
        :key="option.value"
      >
        {{ option.text }}
      </option>
    </select>
    <div class="a-formInput__helpers">
      <span class="a-formInput__error" v-if="errorMsg">{{ errorMsg }}</span>
    </div>
  </div>
</template>
