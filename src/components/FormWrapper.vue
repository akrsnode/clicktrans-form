<script setup>
import { ref, shallowRef } from "@vue/reactivity";
import { watch } from "@vue/runtime-core";
import Textarea from "./inputs/Textarea.vue";
import RadioButton from "./inputs/RadioButton.vue";
import Select from "./inputs/Select.vue";
import TextInput from "./inputs/TextInput.vue";

const textarea = shallowRef(Textarea);
const radioButton = shallowRef(RadioButton);
const select = shallowRef(Select);
const textInput = shallowRef(TextInput);

const fields = ref({
  description: {
    label: "Description",
    fieldComp: textarea,
    maxLength: 255,
    value: "",
    errorMsg: "",
    validate: function () {
      this.errorMsg = "";
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
    fieldComp: radioButton,
    value: null,
    errorMsg: "",
    validate: function () {
      this.errorMsg = "";
      if (this.value === null) {
        this.errorMsg = "Text is required";
        return false;
      }
      return true;
    },
  },
  vat: {
    label: "VAT",
    fieldComp: select,
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
      this.errorMsg = "";
      if (!this.value) {
        this.errorMsg = "Text is required";
        return false;
      }
      return true;
    },
  },
  priceNetto: {
    label: "Price netto EUR",
    fieldComp: textInput,
    disabled: true,
    disabledTooltip: "First choose VAT",
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
    fieldComp: textInput,
    disabled: true,
    disabledTooltip: "You can't edit this field",
    value: null,
  },
});

let activeValidation = false;

const validateFields = () => {
  for (const field of Object.values(fields.value)) {
    field.validate && !field.validate();
  }
};

const handleSubmit = () => {
  activeValidation = true;
  validateFields();
  //todo: mock sending request
};

//todo: omit priceBrutto field
watch(
  () => [
    ...Object.values(fields.value).map(
      (item) => !item.label.includes("brutto") && item.value
    ),
  ],
  () => {
    const { vat, priceNetto, priceBrutto } = fields.value;
    if (vat.value && priceNetto.disabled) {
      priceNetto.disabled = false;
    } else if (priceNetto.value && priceNetto.validate()) {
      priceBrutto.value = +(priceNetto.value * (vat.value / 100 + 1)).toFixed(
        2
      );
    } else {
      priceBrutto.value = "";
    }

    if (activeValidation) {
      validateFields();
    }
  }
);
</script>

<template>
  <h1>Form</h1>
  <form id="form" class="o-form">
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
.o-form {
  width: 80vw;
  max-width: 400px;
}
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
      padding-right: 0.25rem;
      font-weight: 600;
      color: var(--error-color);
    }
    &__textarea,
    &__select,
    &__textInput {
      width: 100%;
    }
    &.-invalid {
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
