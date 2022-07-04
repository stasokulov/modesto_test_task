<template>
  <div class="my-answer">
    <CheckBox class="my-answer__title" :answer="answer" :name="textAreaDisabled ? name : ''" @change="changeInput" />
    <TextArea :textAreaDisabled='textAreaDisabled' :name="name" />
  </div>
</template>

<script>
import CheckBox from '@/components/UI/CheckBox.vue';
import TextArea from '@/components/UI/TextArea.vue';

export default {
    name: "MyAnswer",
    data() {
      return {
        textAreaDisabled: true,
      }
    },
    props: {
        answer: Object,
        name: String,
    },
    methods: {
      changeInput(e) {
        this.textAreaDisabled = !e.target.checked;
      },
    },
    components: {
      CheckBox,
      TextArea,
    }
}
</script>

<style scoped lang="scss">
.my-answer {
  &__title {
    margin-bottom: 12px;
  }
}
</style>
