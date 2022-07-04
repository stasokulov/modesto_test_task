<template>
  <!-- Без аттрибута key элемент textarea не будет меняться при смене одного компонента TextArea на другой. Особенность рендеренга Vue.  -->
  <textarea
    :class="['textarea', {'textarea_disabled': textAreaDisabled}]"
    :name="textAreaDisabled ? '' : name"
    :required="!textAreaDisabled"
    :key="name"
  ></textarea>
</template>

<script>

export default {
  name: 'TextArea',
  props: {
    textAreaDisabled: {
      type: Boolean,
      required: true,
      default: false,
    },
    name: String,
  },
}
</script>

<style scoped lang="scss">
  .textarea {
    box-sizing: border-box;
    width: 259px;
    height: 74px;
    background-color: #F5F5F5;
    border: 2px solid #B3B3B3;
    border-radius: 5px;
    resize: none;

    &_disabled {
      background-color: #E6E6E6;
      pointer-events: none;
    }
  }
</style>
