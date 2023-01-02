<script setup>
import { onMounted, ref } from "@vue/runtime-core";

defineProps({
  message: String,
  success: Boolean,
  error: Boolean,
});
defineEmits("closeDialog");

const isHidden = ref(true);

onMounted(() => {
  setTimeout(() => {
    isHidden.value = false;
  }, 1);
});
</script>

<template>
  <div
    class="o-dialog"
    :class="{ '-hidden': isHidden, '-success': success, '-error': error }"
  >
    <div class="o-dialog__text">
      {{ message }}
    </div>
    <button class="o-dialog__button" @click="$emit('closeDialog')">
      Close
    </button>
  </div>
</template>

<style lang="scss" scoped>
.o-dialog {
  width: 300px;
  min-height: 150px;
  padding: 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: column;
  border-radius: 15px;
  border: 1px solid #000;
  transition: all 0.7s ease-in-out;

  &.-success {
    border-color: var(--success-color);
    color: var(--success-color);
    button {
      color: #fff;
      border-color: var(--success-color);
      background-color: var(--success-color);
    }
  }
  &.-error {
    border-color: var(--error-color);
    color: var(--error-color);
    button {
      color: #fff;
      border-color: var(--error-color);
      background-color: var(--error-color);
    }
  }
  &.-hidden {
    transform: translateY(-100vh);
  }
  &__text {
    font-size: 1.1rem;
    font-weight: 600;
    text-align: center;
    margin-bottom: 1rem;
  }
  &__button {
    width: 100%;
    font-weight: 600;
  }
}
</style>
