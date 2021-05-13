<template>
  <div>
    <v-container>
      <v-row>
        <h1
          @click="resetAll"
          class="app-title"
          title="リセットして最初の質問に戻ります。"
        >
          診断テストのテスト
        </h1>
      </v-row>

      <v-row>
        <v-col cols="8">
          <template v-if="!result">
            <v-card>
              <div class="d-flex justify-space-between">
                <div>
                  <div class="d-inline-block"><v-card-title class="d-inline-blcok">Q{{ currentQuestion.id }}</v-card-title></div>
                  <v-btn class="d-inline-block" v-if="stepNumber !== questions[0].id" @click="backQuestion">前の質問に戻る</v-btn>
                </div>
                <div class="pa-5">あと{{ remainingQuestionsCount }}問</div>
              </div>


              <v-card-subtitle>{{ currentQuestion.title }}</v-card-subtitle>
              <v-card-actions>
                <div
                  v-for="choice in currentQuestion.choices"
                  :key="choice.name"
                  @click="addAnswer(currentQuestion.id, choice.name)"
                  class="choice-button"
                  :class="{ 'choice-button-chosen': chosenAnswer(currentQuestion.id, choice.name) }"
                >
                  <div class="choice-button-image-frame">
                    <img
                      :src="choice.image" :alt="choice.name"
                      class="choice-button-image"
                    >
                  </div>
                  <div class="choice-button-text">
                    {{ choice.name }}
                  </div>
                </div>
              </v-card-actions>
            </v-card>
          </template>

          <template v-else>
            <v-card>
              <v-card-title>診断結果</v-card-title>
              <v-card-text>
                あなたは変な人ですね。
              </v-card-text>
            </v-card>
          </template>
        </v-col>

        <v-col cols="4">
          <v-card>
            <v-card-title>
              選択済みの質問
            </v-card-title>
            <v-card-text>
              <p v-for="answer in answers" :key="answer.questionId">{{ answer.answer }}</p>
            </v-card-text>
          </v-card>

        </v-col>

      </v-row>
    </v-container>
  </div>
</template>





<script>
export default {
  data () {
    return {
      stepNumber: 1,

      questions: [
        {
          id: 1,
          title: "どっちの方が好き？",
          choices: [
            {
              name: "いぬ",
              image: require(`@/assets/pet_omocha_inu.png`),
            },
            {
              name: "ねこ",
              image: require(`@/assets/sleep_animal_cat.png`),
            },
          ]
        },
        {
          id: 2,
          title: "どっちの方が好き？",
          choices: [
            {
              name: "きつね",
              image: require(`@/assets/animalface_kitsune.png`)
            },
            {
              name: "たぬき",
              image: require(`@/assets/animalface_tanuki.png`)
            },
          ]
        },
        {
          id: 3,
          title: "よく食べる方は？",
          choices: [
            {
              name: "カレー",
              image: require(`@/assets/food_curryrice_white.png`)
            },
            {
              name: "ラーメン",
              image: require(`@/assets/ramen_syouyu.png`)
            },
          ]
        },
        {
          id: 4,
          title: "よく見る方は？",
          choices: [
            {
              name: "ドラマ",
              image: require(`@/assets/tv_drama.png`)
            },
            {
              name: "アニメ",
              image: require(`@/assets/kyodai_robot.png`)
            },
          ]
        },
      ], /* questions */

      answers: [],

      result: false,

      answeredQuestions: 0,

      test: 'testtest'
    }
  }, /* data */

  computed: {
    // 現在の質問
    currentQuestion () {
      return this.questions.find((question) => question.id === this.stepNumber)
    },

    // 残りの質問数
    remainingQuestionsCount () {
      return this.questions.length - this.answeredQuestions
    },

    // 選択肢ボタンのスタイル
    style_ChoiceButton () {
      return function (choice) {
        return {
          backgroundImage: 'url(' + choice.image+ ')',

        }
      }
    },

    // 回答済みの選択をマークする
    chosenAnswer () {
      return function (questionId, answer) {
        const self = this
        const answeredQuestion = self.answers.find(answer => answer.questionId === questionId)
        if (answeredQuestion) {
          if(answeredQuestion.answer === answer) {
            return true
          } else {
            return false
          }
        } else {
           return false
        }
      }
    },

  }, /* computed */


  methods: {
    resetAll () {
      this.stepNumber = 1
      this. result = false
      this.answeredQuestions=  0
      this.answers = []
    },

    // 質問に回答
    addAnswer (questionId, newAnswer) {
      let answeredQuestion = this.answers.find(answer => answer.questionId === questionId)
      if(answeredQuestion) {
        const index = this.answers.findIndex((v) => v.questionId === answeredQuestion.questionId)
        // 回答を置き換え
        this.answers.splice(index, 1, {
          questionId: questionId,
          answer: newAnswer
        })
      } else {
        // 回答を追加
        this.answers.push({questionId: questionId, answer: newAnswer})
      }

      if(this.stepNumber !== this.questions.length) {
        // 次の質問へ
        this.stepNumber++
        this.answeredQuestions++
      } else {
        // 結果表示へ
        this.result = true
      }
    },

    // 前の質問に戻る
    backQuestion () {
      if(this.stepNumber !== this.questions[0].id) {
        this.stepNumber--
        this.answeredQuestions--
      }
    },

  }, /* methods */
}
</script>





<style scoped lang="scss">
img {
  max-width: 100%;
  height: auto;
}

.app-title {
  cursor: pointer;
}

.choice-button {
  $main-color: #456;
  $border-radius: 10px;
  $border: 4px solid $main-color;
  $text-area-height: 2.2em;

  border:$border;
  height: 220px;

  @media screen and (max-width: 600px) {
    height: 110px;
  }

  width: 50%;
  cursor: pointer;
  position: relative;
  border-radius: $border-radius;
  user-select: none;
  text-align: center;

  &:not(:first-child) {
    margin-left: 1rem;
  }

  &:hover {
    box-shadow: 0px 0px 10px blue;
  }
  &-chosen {
    box-shadow: 0px 0px 10px blue;
  }

  .choice-button-image-frame {
    height: calc(100% - #{$text-area-height});
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2%;
  }
  .choice-button-image {
    object-fit: cover;
    max-height: 100%;
  }

  .choice-button-text {
    height: $text-area-height;
    background-color: #fff;
    width: 100%;
    text-align: center;
    position: absolute;
    bottom: 0;
    background-color: $main-color;
    padding: 0.6em 0 0.4em;
    color: #fff;
    font-weight: bold;
    font-family: Meirio;
    letter-spacing: 2px;
  }
}
</style>
