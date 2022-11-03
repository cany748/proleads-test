<script setup></script>

<template>
  <header class="header">
    <div class="header__wrapper block-wrapper">
      <img
        class="header__logo"
        src="src/assets/img/logo-small-dark.svg"
        alt="Logo"
      />
    </div>
  </header>
</template>

<style scoped lang="scss">
.header {
  padding: 64px 0;
  &__logo {
    width: 204px;
    height: 56px;
    margin-left: 99px;
  }
}

@media (max-width: 1240px) {
  .header {
    &__wrapper {
      padding: 0 16px;
    }
    &__logo {
      margin-left: 0;
    }
  }
}

@media (max-width: 768px) {
  .header {
    padding-top: 16px;
  }
}

@media (max-width: 680px) {
  .header {
    &__wrapper {
      padding: 0 8px;
    }
    &__logo {
      width: 146px;
      height: 40px;
    }
  }
}
</style>
