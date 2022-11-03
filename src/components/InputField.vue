<script setup>
import { defineProps, ref } from "vue";
import { useField } from "vee-validate";

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  type: {
    type: String,
    default: "text",
  },
  rules: {
    type: null,
    default: null,
  },
  value: {
    type: String,
    default: "",
  },
  filter: {
    type: Function,
    default: () => {},
  },
  title: {
    type: String,
    default: "",
  },
  filterErrorText: {
    type: String,
    default: "",
  },
});

const input = ref(null);
const filterError = ref(false);

function focusInput() {
  input.value.focus();
}

const {
  value: inputValue,
  handleBlur,
  handleChange,
  errorMessage,
  meta,
} = useField(props.name, props.rules, {
  initialValue: props.value,
});
</script>

<template>
  <div
    class="field"
    :class="{
      'is-filled': inputValue,
      'is-error': (errorMessage || filterError) && !meta.valid,
    }"
    @click="focusInput"
  >
    <div class="field__title">{{ title }}</div>
    <input
      :name="name"
      :type="type"
      class="field__input"
      :value="inputValue"
      ref="input"
      @input="handleChange"
      @blur="handleBlur"
      @beforeinput="filter($event, this)"
    />
    <div class="field__error">{{ errorMessage || filterErrorText }}</div>
    <button class="field__clear" type="button" @click="inputValue = null">
      <img src="src/assets/img/subtract.svg" alt="" />
    </button>
  </div>
</template>

<style scoped lang="scss">
.field {
  width: 100%;
  box-sizing: border-box;
  position: relative;
  background: $color-neutral-2;
  border: 1px solid $color-neutral-3;
  transition: $transition-main;
  border-radius: 32px;
  &:hover {
    border-color: $color-brand-primary;
  }
  &:focus-within {
    background: $color-neutral-1;
    border-color: $color-brand-primary;
  }
  &:focus-within,
  &.is-filled {
    .field__title {
      @include lable-small;
      color: $color-brand-primary;
      transform: none;
      top: 6px;
    }
  }
  &.is-filled {
    .field__clear {
      opacity: 1;
      pointer-events: all;
    }
  }
  &.is-warning {
    background: $color-neutral-1;
    border-color: $color-brand-tertiary;
  }
  &.is-error {
    background: $color-neutral-1;
    border-color: $color-brand-secondary;

    .field__error {
      opacity: 1;
    }
  }
  &.is-disabled {
    background: $color-neutral-2;
    border-color: $color-neutral-3;
    pointer-events: none;
    .field__title {
      color: $color-neutral-3;
    }
    .field__error {
      opacity: 0;
    }
  }
}
.field__input {
  @include paragraph-regular;
  width: 100%;
  display: block;
  box-sizing: border-box;
  color: $color-neutral-6;
  outline: 0;
  appearance: none;
  border: none;
  background: none;
}
.field__title {
  @include paragraph-regular;
  position: absolute;
  overflow: hidden;
  pointer-events: none;
  transform: translateY(-50%);
  transition: $transition-main;
  top: 50%;
}
.field__error {
  @include lable-small;
  font-size: 10px;
  position: absolute;
  overflow: hidden;
  transition: $transition-main;
  color: $color-brand-secondary;
  opacity: 0;
  bottom: 0;
}
.field__clear {
  width: 23px;
  height: 19px;
  position: absolute;
  background: none;
  cursor: pointer;
  border: none;
  transition: $transition-main;
  transform: translateY(-50%);
  padding: 0;
  opacity: 0;
  pointer-events: none;
  right: 31px;
  top: 50%;
}
</style>
