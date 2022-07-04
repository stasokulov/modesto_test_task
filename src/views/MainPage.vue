<template>
  <div>
    <Header :menu="menu" />
    <button @click="clearLS">Сбросить LocalStorage</button>
    <Popup v-if="popupShow" @closePopup="closePopup">
      <Quiz @close="closePopup"/>
    </Popup>
  </div>
</template>

<script>
import Header from '@/components/Header.vue';
import Popup from '@/components/Popup.vue';
import Quiz from '@/components/Quiz.vue';

export default {
  name: "MainPage",
  data() {
    return {
      menu: ['Home', 'Catalog', 'Tenders', 'Trade', 'Web-Shop NL'],
      popupShow: true,
    }
  },
  methods: {
    closePopup() {
      this.popupShow = false;
      window.localStorage.setItem('quizClosed', "true");
    },
    clearLS() {
      window.localStorage.setItem('quizClosed', "false")
    },
  },
  mounted() { 
    this.popupShow = window.localStorage.getItem('quizClosed') === "false";
  },
  components: { Header, Popup, Quiz }
}
</script>

<style scoped lang="scss">

</style>
