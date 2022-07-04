<template>
  <div class="header">
    <div class="header__content">
      <a class="header__logo" href="https://m-flowers.com/" target="_blank">
        <img alt="logo" src="@/assets/logo.svg">
      </a>
      <ul class="header__menu">
        <li
          v-for="item in menu"
          :key="item"
          class="header__menu-item"
          @click="showAlert"
        >
          {{item}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  props: {
    menu: Array,
  },
  methods: {
    showAlert() {
      alert('Здесь могла быть ваша реклама');
    }
  }
}
</script>

<style scoped lang="scss">
.header {
  background: #C4C4C4;
  height: 60px;
  width: 100%;

  &__content {
    display: flex;
    justify-content: flex-start;
    max-width: 1000px;
    height: 100%;
    margin: auto;
  }

  &__logo {
    display: flex;
    align-items: center;
  }

  &__menu {
    display: flex;
    align-items: center;
    margin: 0;
    padding: 0 0 0 95px;
    list-style-type: none;
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 21px;
  }

  &__menu-item {
    cursor: pointer;

    &:not(:last-child) {
      margin-right: 38px;
    }
  }
}
</style>
