<script setup>
import { ref } from "vue";
import { Form } from "vee-validate";
import InputField from "../InputField.vue";

const contacts = ref(null);

function onSubmit(values) {
  console.log("Form valid:", values);
  contacts.value.hidden = true;
  alert("Форма успешно заполнена");
}
function onInvalidSubmit() {
  console.log("Form INVALID");
}
function validateRequired(value) {
  if (!value) {
    return "Это поле необходимо заполнить";
  } else {
    return true;
  }
}
function validateDob(value) {
  const regex = /[0-9]{2}.[0-9]{2}.[0-9]{4}/i;
  if (value.length !== 10 || !regex.test(value)) {
    return "Введите корректную дату в формате дд.мм.гггг";
  }
  const [day, month, year] = value.split(".");
  const date = new Date(+year, +month - 1, +day);
  date.setFullYear(date.getFullYear() + 18);
  if (date > new Date()) {
    return "Меньше 18 лет!";
  }
  return true;
}
function validateEmail(value) {
  const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
  if (!regex.test(value)) {
    return "Введите корректный E-mail";
  }
  return true;
}
function filterDob(event) {
  const char = event.data;
  if (!char || /[0-9]|\./i.test(char)) {
    return true;
  } else {
    event.preventDefault();
    event.stopPropagation();
  }
}
function filterText(event, $this) {
  const char = event.data;
  if (!char || /^[А-ЯЁ]*$/i.test(char)) {
    $this.filterError = false;
    return true;
  } else {
    $this.filterError = true;
    event.preventDefault();
    event.stopPropagation();
  }
}
</script>

<template>
  <main class="content">
    <div class="content__wrapper block-wrapper">
      <section class="contacts" ref="contacts">
        <h1 class="contacts__title">Введите личные данные</h1>
        <div class="contacts__subtitle">
          Будьте внимательны при вводе данных
        </div>
        <Form
          class="contacts__form form"
          @submit="onSubmit"
          @invalid-submit="onInvalidSubmit"
          v-slot="{ meta }"
        >
          <InputField
            class="form__field"
            name="surname"
            type="text"
            :rules="[validateRequired]"
            :filter="filterText"
            filterErrorText="Только кирилические буквы"
            title="Фамилия"
          />
          <div class="form__row">
            <InputField
              class="form__field"
              name="name"
              type="text"
              :rules="[validateRequired]"
              :filter="filterText"
              filterErrorText="Только кирилические буквы"
              title="Имя"
            />
            <InputField
              class="form__field"
              name="patronymic"
              type="text"
              :rules="[validateRequired]"
              :filter="filterText"
              filterErrorText="Только кирилические буквы"
              title="Отчество"
            />
          </div>
          <InputField
            class="form__field"
            name="dob"
            type="text"
            :rules="[validateRequired, validateDob]"
            :filter="filterDob"
            title="Дата рождения"
          />
          <InputField
            class="form__field"
            name="address"
            type="text"
            :rules="[validateRequired]"
            :filter="filterText"
            filterErrorText="Только кирилические буквы"
            title="Место проживания"
          />
          <InputField
            class="form__field"
            name="email"
            type="email"
            :rules="[validateRequired, validateEmail]"
            title="E-mail"
          />
          <button
            class="form__submit"
            :class="{ 'is-disabled': meta.touched && !meta.valid }"
          >
            Продолжить
          </button>
        </Form>
      </section>
    </div>
  </main>
</template>

<style scoped lang="scss">
.contacts {
  width: 988px;
  box-sizing: border-box;
  margin: 0 auto;
  &__title {
    width: 611px;
    background: $color-neutral-6;
    @include title-hero;
    color: $color-neutral-1;
    margin: 0 0 24px;
  }
  &__subtitle {
    position: relative;
    @include paragraph-lead;
    color: $color-lable-disabled;
    padding-left: 16px;
    margin-bottom: 24px;
    &::before {
      content: "";
      display: block;
      width: 4px;
      height: 100%;
      position: absolute;
      background: $color-brand-primary;
      border-radius: 2px;
      top: 0;
      left: 0;
    }
  }
}
.form {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 16px;
  height: 664px;
  position: relative;
  background: #c4cbd0;
  box-shadow: 0px 12px 24px rgba(16, 25, 32, 0.08);
  box-sizing: border-box;
  border-radius: 32px;
  padding: 48px 179px;
  margin-bottom: 584px;
  &::before {
    content: "";
    display: block;
    position: absolute;
    width: calc(100% - 3px);
    height: calc(100% - 8px);

    background-color: $color-neutral-6;
    border-radius: 32px;
    z-index: -1;
    top: 32px;
    left: 27px;
  }
  &::after {
    content: "";
    display: block;
    position: absolute;
    width: 357px;
    height: 808px;
    background: url("src/assets/img/girl.svg");
    background-size: 100%;
    right: -202px;
    bottom: -588px;
  }
  &__field {
    height: 64px;
    padding: 20px 32px;
  }
  &__row {
    width: 100%;
    display: flex;
    justify-content: space-between;
    gap: 24px;
  }
  &__submit {
    width: calc(50% - 12px);
    height: 64px;
    @include paragraph-medium;
    color: $color-neutral-6;
    font-size: 14px;
    text-align: center;
    text-transform: uppercase;
    transition: $transition-main;
    cursor: pointer;
    border: none;
    border-radius: 30px;
    background: $color-brand-tertiary;
    box-shadow: 0px 12px 30px rgba(255, 182, 6, 0.6);
    margin-left: auto;
    &:hover {
      background: $color-brand-tertiary-hover;
      box-shadow: 0px 8px 16px rgba(255, 182, 6, 0.8);
    }
    &:focus {
      background: $color-brand-tertiary-hover;
      box-shadow: 0px 3px 6px rgba(255, 182, 6, 0.7);
    }
    &.is-disabled,
    &[disabled] {
      background: $color-neutral-2;
      box-shadow: none;
      color: $color-neutral-3;
      cursor: not-allowed;
    }
  }
}
@media (max-width: 1240px) {
  .form {
    margin-bottom: 472px;
    &::after {
      width: 238px;
      height: 538px;
      right: -77px;
      bottom: -474px;
    }
  }
}
@media (max-width: 1140px) {
  .content__wrapper {
    padding-right: 77px;
    padding-left: 77px;
  }
  .contacts {
    width: 100%;
  }
  .form {
    padding-right: 102px;
    padding-left: 102px;
  }
}
@media (max-width: 896px) {
  .content__wrapper {
    padding-right: 35px;
    padding-left: 35px;
  }
  .form {
    padding-right: 90px;
    padding-left: 90px;
    &__row {
      gap: 16px;
    }
    &__submit {
      width: calc(50% - 8px);
    }
    &::after {
      right: -35px;
    }
  }
}

@media (max-width: 768px) {
  .form {
    margin-bottom: 128px;
    &::after {
      content: none;
    }
  }
}

@media (max-width: 680px) {
  .contacts {
    &__title {
      @include title-extra-large;
      width: 460px;
      margin-bottom: 16px;
    }
    &__subtitle {
      @include paragraph-regular;
      padding-left: 10px;
    }
  }
  .form {
    height: 698px;
    gap: 24px;
    padding-top: 32px;
    margin-bottom: 326px;
    &__field {
      height: 56px;
      padding: 16px 32px;
    }
    &__row {
      flex-direction: column;
      gap: 24px;
    }
    &__submit {
      width: 100%;
    }
  }
}

@media (max-width: 530px) {
  .content__wrapper {
    padding-right: 8px;
    padding-left: 8px;
  }
  .contacts__title {
    width: 100%;
  }
  .form::before {
    content: none;
  }
}
@media (max-width: 480px) {
  .form {
    padding: 32px 8px 0 8px;
  }
}
</style>
