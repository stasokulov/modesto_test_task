<template>
  <div class="quiz">
    <div class="quiz__progress">
      <div
        v-for="num in uniqNumArr"
        :key="num"
        :class="['quiz__progress-item', {'quiz__progress-item_active': currentQuest.num >= num}]"
      >
      </div>
    </div>
    <form v-if="currentQuest && currentQuest.answers" class="quiz__quest" @submit.prevent="catchSubmit">
      <h3 class="quiz__title">{{questTitle}}</h3>
      <ul class="quiz__answers">
        <li
          v-for="answer in currentQuest.answers"
          :key="answer.title"
          class="quiz__answer"
        >
          <component
            :is="getAnswerComponent(answer.type)"
            :answer="answer"
            :name="`${currentQuest.id}`"
          ></component>
        </li>
      </ul>
      <Button :title="buttonTitle" type="submit" />
    </form>
    <div v-else class="quiz__preloader">
      <div class="quiz__preloader-img"></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import constants from '@/constans';
import db from '@/db.json'

import Radio from '@/components/UI/Radio.vue';
import CheckBox from '@/components/UI/CheckBox.vue';
import DoNotWantAnswer from '@/components/answers/DoNotWantAnswer.vue';
import MyAnswer from '@/components/answers/MyAnswer.vue';
import Button from '@/components/UI/Button.vue'

export default {
  name: 'Quiz',
  data() {
    return {
      questions: null,
      currentQuestId: null,
      constants: constants,
      result: {},
      urlGet: 'https://run.mocky.io/v3/05429040-2827-46be-a301-317f0f66f3e8', //Бесплатный сервис для храниения JSON.
      urlPost: 'https://run.mocky.io/v3/849fdad1-c328-442a-a88b-500542269c9d',
    }
  },
  computed: {
    uniqNumArr() {
       return this.questions?.length ? [...new Set(this.questions.map(quest => quest.num))] : [];
    },
    currentQuest() {
      return this.questions?.find(quest => quest.id === this.currentQuestId);
    },
    questTitle() {
      return this.currentQuest?.title ?? 'Вопрос не найден';
    },
    buttonTitle() {
      return this.currentQuest?.nextStepId === 'end' ? 'submit' : 'next';
    },
  },
  methods: {
    getAnswerComponent(type) {
      switch(type) {
        case this.constants.RADIO:
          return Radio;
        case this.constants.CHECKBOX:
          return CheckBox;
        case this.constants.TEXTAREAORCHECK:
          return DoNotWantAnswer;
        case this.constants.TEXTAREAANDCHECK:
          return MyAnswer;
      }
    },
    catchSubmit(e) {
      this.addResult(e);
      this.nextAction();
    },
    addResult(e) {
      const formData = new FormData(e.srcElement);
      // Достаем ответы пользователя (value) из инпутов с параметром name = this.currentQuest.id с помощью formData.getAll().
      this.result[this.currentQuest.id] = {
        title: this.currentQuest.title,
        answer: formData.getAll(this.currentQuest.id),
      };
    },
    nextAction() {
      const nextStepId = this.getNextStepId();
      if( Number.isInteger(+nextStepId) ) {
        this.goNextStep(nextStepId);
      } else if (nextStepId === 'end') {
        this.sendResult();
        this.$emit('close');
      } else (
        alert('Ошибка в номере вопроса. Обратитесь в техподдержку.')
      )
    },
    goNextStep(id) {
      this.currentQuestId = id;
    },
    getNextStepId() {
      // results -> находим вопрос c ответом по id текущего впроса -> берем текст первого ответа из массива ответов -> использем его для поиска объекта ответа в текущем вопросе -> получаем nextStep из ответа.
      // Предполагаем, что nextStep может быть только у вопроса с единственным вариантом ответа (единственной строкой в массиве this.result[this.currentQuest.id].answer), т.е. у ответа с радиокнопкой.
      const currentAnswerTitle = this.result
        && this.result[this.currentQuest.id]
        && this.result[this.currentQuest.id].answer
        && this.result[this.currentQuest.id].answer.length > 0
        ? this.result[this.currentQuest.id].answer[0]
        : null;
      const currentAnswer = this.currentQuest.answers.find(answer => answer.title === currentAnswerTitle);
      return (!!currentAnswer && currentAnswer.nextStepId) || this.currentQuest.nextStepId;
    },
    sendResult() {
      axios.post(this.urlPost, this.result)
      .then(response => console.log('response', response))
      .catch(error => console.log('error', error));
    }
  },
  async mounted() {
    try{
      const questions = await axios.get(this.urlGet);
      this.questions = questions.data;
    } catch {
      this.questions = db;
    }
    this.currentQuestId = this.questions[0].id;
  },
  components: {
    Radio,
    CheckBox,
    DoNotWantAnswer,
    MyAnswer,
    Button,
  }
}
</script>

<style scoped lang="scss">
.quiz {
  &__progress {
    display: flex;
    height: 21px;
    margin-bottom: 22px;
  }

  &__progress-item {
    position: relative;
    flex-shrink: 1;
    width: 100%;
    min-width: 21px;

    &:first-child {
      flex-shrink: 2;
    }

    &::before,
    &::after {
      content: '';
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
    }

    &::before {
      right: 0;
      left: 0;
      height: 3px;
      background-color: #B3B3B3;
    }

    &::after {
      right: 0;
      box-sizing: border-box;
      width: 21px;
      height: 21px;
      background: #C4C4C4;
      border: 2px solid #B3B3B3;
      border-radius: 50%;
    }

    &_active {
      &::before {
        background-color: #4BBBFA;
      }

      &::after {
        background: #8CD6FF;
        border-color: #4BBBFA;
      }
    }
  }

  &__title {
    margin: 0 0 30px;
  }

  &__answers {
    list-style: none;
    margin: 0 0 30px;
    padding: 0;
  }

  &__answer {
    font-style: normal;
    font-weight: 400;
    font-size: 12px;
    line-height: 18px;
    color: #000000;

    &:not(:last-child) {
      margin-bottom: 12px;
    }
  }

  &__preloader {
    animation-duration: 3s;
    animation-name: rotate;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
  }

  @keyframes rotate {
    from {
      transform: rotate(0deg);
      // font-size: 10px;
    }
    to {
      transform: rotate(360deg);
      // font-size: 100px;
    }
  }

  &__preloader-img {
    width: 0;
    height: 0;
    margin: auto;
    border-style: solid;
    border-color: #000000;
    border-top-width: 40px;
    border-bottom-width: 40px;
    border-right-width: 25px;
    border-left-width: 25px;
    border-right-color: transparent;
    border-left-color: transparent;
  }
}
</style>
