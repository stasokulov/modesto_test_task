<template>
  <div class="not-want-answer">
    <TextArea :textAreaDisabled='textAreaDisabled' :name="name" required />
    <CheckBox :answer="answer" :name="name" @change="changeInput" />
  </div>
</template>

<script>
import CheckBox from '@/components/UI/CheckBox.vue';
import TextArea from '@/components/UI/TextArea.vue';

export default {
    name: "doNotWantAnswer",
    data() {
      return {
        textAreaDisabled: false,
      }
    },
    props: {
        answer: Object,
        name: String,
    },
    methods: {
      changeInput(e) {
        this.textAreaDisabled = e.target.checked;
      },
    },
    components: {
      CheckBox,
      TextArea,
    }
}
</script>

<style scoped lang="scss">
.not-want-answer {
  &__textarea {
    margin-bottom: 12px;
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
}
</style>
