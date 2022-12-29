<script setup>
import { ref } from "@vue/reactivity";
import { watch } from "@vue/runtime-core";
import Textarea from "./inputs/Textarea.vue";
import RadioButton from "./inputs/RadioButton.vue";
import Select from "./inputs/Select.vue";
import TextInput from "./inputs/TextInput.vue";

const fields = ref({
  description: {
    label: "Description",
    fieldComp: Textarea,
    maxLength: 255,
    value: "",
    errorMsg: "",
    validate: function () {
      if (!this.value) {
        this.errorMsg = "Text is required";
        return false;
      } else if (this.value.length > this.maxLength) {
        this.errorMsg = `You can’t enter more than 255 characters`;
        return false;
      }
      return true;
    },
  },
  confirmation: {
    label: "Send confirmation",
    fieldComp: RadioButton,
    value: null,
    errorMsg: "",
    validate: function () {
      if (!this.value) {
        this.errorMsg = "Text is required";
        return false;
      }
      return true;
    },
  },
  vat: {
    label: "VAT",
    fieldComp: Select,
    placeholder: "Choose VAT",
    options: [
      { value: 19, text: "19%" },
      { value: 21, text: "21%" },
      { value: 23, text: "23%" },
      { value: 25, text: "25%" },
    ],
    value: null,
    errorMsg: "",
    validate: function () {
      if (!this.value) {
        this.errorMsg = "Text is required";
        return false;
      }
      return true;
    },
  },
  priceNetto: {
    label: "Price netto EUR",
    fieldComp: TextInput,
    disabled: true,
    value: null,
    errorMsg: "",
    validate: function () {
      this.errorMsg = "";
      if (!this.value || Number.isNaN(this.value * 1)) {
        this.errorMsg = "Please, input number";
        return false;
      }
      return true;
    },
  },
  priceBrutto: {
    label: "Price brutto EUR",
    fieldComp: TextInput,
    disabled: true,
    value: null,
  },
});

watch(
  fields,
  () => {
    console.log("🚩: watch callback");
    const { vat, priceNetto, priceBrutto } = fields.value;
    if (vat.value && priceNetto.disabled) {
      priceNetto.disabled = false;
    } else if (priceNetto.value && priceNetto.validate()) {
      priceBrutto.value = priceNetto.value * (vat.value / 100 + 1);
    }
  },
  { deep: true }
);

let invalidForm = false;

const handleSubmit = () => {
  for (const [key, field] of Object.entries(fields.value)) {
    if (field.validate && !field.validate()) invalidForm = true;
    //todo: then on change revalidate
  }
  console.log("🚩: ", "Send form to the moon");
};
</script>

<template>
  <h1>Formularz</h1>
  <form id="form">
    <component
      v-for="field in fields"
      :key="field.label"
      :is="field.fieldComp"
      v-bind="field"
      v-model="field.value"
    />
    <button
      type="submit"
      form="form"
      value="Submit"
      class="a-formButton"
      @click.prevent="handleSubmit"
    >
      Submit
    </button>
  </form>
</template>

<style lang="scss">
.a-form {
  &Input {
    margin-bottom: 2rem;
    display: flex;
    flex-flow: column nowrap;
    align-items: flex-start;
    &__label {
      margin-bottom: 0.5rem;
      font-size: 1.1rem;
    }
    &__helpers {
      margin-top: 0.25rem;
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      justify-content: space-between;
      font-size: 0.8rem;
      opacity: 0.8;
    }
    &__option {
      display: flex;
      align-items: baseline;
      margin: 0.25rem 0;
      &Label {
        padding-left: 0.125rem;
      }
    }
    &__error {
      font-weight: 600;
      color: var(--error-color);
    }
    &__textarea,
    &__select {
      width: 100%;
    }
    &.-invalid {
      /* color: var(--error-color); */
      textarea,
      input {
        border-color: var(--error-color);
      }
    }
  }
  &Button {
    width: 100%;
    padding: 0.5rem 1.75rem;
    font-size: 1rem;
    font-weight: 600;
    color: #fff;
    background-color: var(--brand-color);
    border-color: var(--brand-color);
  }
}
</style>
