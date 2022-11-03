<script setup></script>

<template>
  <footer class="footer">
    <div class="footer__wrapper block-wrapper">
      <img
        class="footer__logo"
        src="src/assets/img/logo-small-light.svg"
        alt="Logo"
      />

      <div class="footer__side">
        <div class="footer__notice notice">
          <div class="notice__text">
            Внимание! Услуга <a href="#">обработки заявки платная</a><br />
            Imoneys ©2015–2021 Все права на сайт защищены.
          </div>
          <img class="notice__icon" src="src/assets/img/notice.svg" alt="" />
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped lang="scss">
.footer {
  background: $color-neutral-6;
  margin-top: auto;
  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 16px;
    padding-bottom: 16px;
  }
}

.notice {
  display: flex;
  justify-content: space-between;
  align-items: center;
  &__text {
    @include lable-small;
    text-align: right;
    color: $color-lable-paragraph;
    margin-right: 16px;
    a {
      color: $color-brand-primary;
      transition: $transition-main;
      &:hover {
        color: $color-brand-primary-hover;
      }
    }
  }
  &__icon {
    width: 32px;
    height: 32px;
  }
}

@media (max-width: 1024px) {
  .footer__logo {
    width: 147px;
    height: 40px;
  }
}
@media (max-width: 600px) {
  .footer {
    &__wrapper {
      flex-direction: column;
      gap: 16px;
      padding-left: 8px;
      padding-right: 8px;
    }
    &__logo {
      margin-right: auto;
    }
    &__side {
      margin-left: auto;
    }
  }
}
@media (max-width: 400px) {
  .footer__side {
    width: 100%;
    margin-left: 0;
    margin-right: auto;
  }
  .notice__text {
    text-align: left;
    margin-right: 10px;
  }
}
</style>
