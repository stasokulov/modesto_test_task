<template>
  <div class="popup">
    <div class="popup__content">
      <div class="popup__close" @click="$emit('closePopup')"></div>
      <h2 class="popup__title">Please answer a few questions</h2>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Popup',
}
</script>

<style scoped lang="scss">
.popup {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.6);

  &__content {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 600px;
    background: #F5F5F5;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    border-radius: 5px;
    padding: 32px 40px 40px;
  }

  &__close {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 14px;
    height: 14px;
    cursor: pointer;
    background-image: url("data:image/svg+xml,%3Csvg width='14' height='14' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='m14 .8-.8-.8L7 6.2.8 0 0 .8 6.2 7 0 13.2l.8.8L7 7.8l6.2 6.2.8-.8L7.8 7 14 .8Z' fill='%23000'/%3E%3C/svg%3E");
  }

  &__title {
    font-style: normal;
    font-weight: 700;
    font-size: 18px;
    line-height: 30px;
    margin: 0 auto 51px;
  }
}
</style>
