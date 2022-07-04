<template>
  <!-- <button class="btn" :type="type" @click.prevent="btnClick"> -->
    <button class="btn" :type="type" @click="btnClick">
    {{title}}
  </button>
</template>

<script>

export default {
  name: 'Button',
  props: {
    title: {
      type: String,
      required: true,
      validator(value) {
        return ['submit', 'next'].includes(value)
      }
    }
  },
  computed: {
    type() {
      return this.title === 'submit' ? 'submit' : 'button';
    }
  },
  methods: {
    btnClick() {
      this.title === 'submit' ? this.$emit('submit') : this.$emit('click');
    }
  }
}
</script>

<style scoped lang="scss">
.btn {
  width: 133px;
  height: 40px;
  background-color: #4BBDFD;
  border-radius: 5px;
  font-style: normal;
  font-weight: 700;
  font-size: 14px;
  line-height: 18px;
  color: #FFFFFF;
  text-transform: capitalize;
  border: none;
  cursor: pointer;
}
</style>
