<template>
  <label class="custom-input custom-input_radio">
    <input
      class="custom-input__input"
      type="radio"
      :name="name"
      :value="answer.title"
      required
      @change="emitChange"
    />
    <div class="custom-input__custom-input"></div>
    <span>{{answer.title}}</span>
  </label>
</template>

<script>

export default {
  name: 'Radio',
  props: {
    name: String,
    answer: Object,
  },
  methods: {
    emitChange(e) {
      this.$emit('change', e);
    }
  }
}
</script>

<style scoped lang="scss">
.custom-input {
  display: flex;
  align-items: center;

  &__input {
    position: absolute;
    width: 1px;
    height: 1px;
    z-index: -1;
    opacity: 0;
  }
  
  &__custom-input {
    position: relative;
    width: 18px;
    height: 18px;
    margin-right: 12px;

    &::before,
    &::after {
      content: '';
      position: absolute;
      top: 0;
      bottom: 0;
      right: 0;
      left: 0;
    }
  }

  &_radio &__custom-input{
    border-radius: 50%;

    &::before {
      background-image: url("data:image/svg+xml,%3Csvg width='18' height='18' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Ccircle cx='9' cy='9' r='8' fill='%23E6E6E6' stroke='%23B3B3B3' stroke-width='2'/%3E%3C/svg%3E");
    }
  }

  &_radio &__input:checked + &__custom-input::after {
    background-image: url("data:image/svg+xml,%3Csvg width='18' height='18' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Ccircle cx='9' cy='9' r='8' fill='%23BFBFBF' stroke='%23B3B3B3' stroke-width='2'/%3E%3Cpath d='M14 5.2 7.5 10 4 7.3v1.3l3.5 4.2L14 6.5V5.2Z' fill='%23fff'/%3E%3C/svg%3E");
  }
}
</style>
