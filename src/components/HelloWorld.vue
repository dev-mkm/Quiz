<script>
  export default {
    data() {
      return {
        name: "",
        idx: -1,
        selectedAnswer: "",
        correctAnswers: 0,
        wrongAnswers: 0,
        count: 3,
        countDown: 60,
        questions: [
          {
            question:
              "رولکس شرکتی است که در چه نوع محصولی تخصص دارد؟",
            answers: { a: "کیف", b: "ساعت", c: "کفش", d: "لپتاپ" },
            correctAnswer: "b",
          },
          {
            question: "فیس بوک چه زمانی راه اندازی شد؟",
            answers: { a: "2005", b: "2008", c: "2003", d: "2004" },
            correctAnswer: "d",
          },
          {
            question:
              "آلبرت انیشتین زمانی که در مدرسه بود با ریاضیات مشکل داشت؟",
            answers: { a: "بله", b: "خیر" },
            correctAnswer: "b",
          },
        ],
      };
    },
    methods: {
      start() {
        if(this.name != '') {
          this.idx = 0;
          this.countDown = 60;
          this.countDownTimer()
        }
      },
      countDownTimer () {
        if(this.selectedAnswer == '') {
          if (this.countDown > 0) {
            setTimeout(() => {
              this.countDown -= 1
              this.countDownTimer()
            }, 1000)
          } else {
            this.selectedAnswer = 'wrong'
            this.wrongAnswers++;
          }
        }
      },
      answered(e) {
        this.selectedAnswer = e;
        if (this.selectedAnswer == this.questions[this.idx].correctAnswer) {
          this.correctAnswers++;
        } else {
          this.wrongAnswers++;
        }
      },
      skip() {
        this.selectedAnswer = 'wrong'
        this.wrongAnswers++;
      },
      nextQuestion() {
        this.idx++;
        this.selectedAnswer = "";
        this.countDown = 60;
        this.countDownTimer()
      },
      showResults() {
        this.idx++;
      },
      resetQuiz() {
        this.idx = -1;
        this.selectedAnswer = "";
        this.correctAnswers = 0;
        this.countDown = 60;
        this.wrongAnswers = 0;
      },
    }
  }
</script>

<template>
  <v-container class="fill-height" style="max-width: 700px;">
    <v-responsive v-if="idx == -1" class="align-center text-center fill-height">
      <v-avatar image="../../public/icon.png" size="200"></v-avatar>
      <h1 class="text-h3 font-weight-bold">به مسابقه بزرگ ماه خوش آمدید</h1>

      <div class="py-2" />

      <div class="text-h5 font-weight-light">لطفا اطلاعات خود را وارد کنید</div>

      <div class="py-10" />

      <v-text-field label="نام و نام خانوادگی" variant="outlined" v-model="name"></v-text-field>
      <v-btn class="w-100 mcolor" @click="start()" :class="{'disable-events': name == ''}">
        شروع مسابقه
      </v-btn>
    </v-responsive>
    <v-responsive v-else-if="idx < count" class="align-center text-center fill-height">
      <v-progress-linear
        :model-value="countDown * 100 / 60"
        height="25"
        color="#52b788"
        rounded
      >
        <strong>{{ countDown > 0 ? countDown + ' ثانیه' : 'زمان به اتمام رسید' }}</strong>
      </v-progress-linear>
      <div class="py-10" />
      <h1 class="text-h4 font-weight-bold text-right">سوال {{ idx+1 }}/{{ count }}</h1>
      <div class="py-5" />
      <v-card :title="questions[idx]['question']" style="background: aliceblue;" class="pa-8 text-right">
        <v-btn v-for="(answer, index) in questions[idx].answers" @click="answered(index)" class="w-100 mt-4"
        :key="index" :class="{'disable-events': selectedAnswer != ''}, {'green' : index == questions[idx].correctAnswer && selectedAnswer != ''}, {'red' : (index != questions[idx].correctAnswer && selectedAnswer == index) || (index != questions[idx].correctAnswer && selectedAnswer == 'wrong')}" variant="outlined">
          {{answer}}
        </v-btn>
        <div class="pt-6 d-flex justify-space-between">
          <v-btn @click="skip()" :disabled="selectedAnswer != ''" variant="text">
            رد شدن
          </v-btn>
          <v-btn class="mcolor" @click="nextQuestion" v-show="selectedAnswer != '' && idx < count - 1" variant="text">
            سوال بعد
          </v-btn>
          <v-btn class="mcolor" @click="showResults" v-show="selectedAnswer != '' && idx == count - 1" variant="text">
            اتمام مسابقه
          </v-btn>
        </div>
      </v-card>
    </v-responsive>
    <v-responsive v-else class="align-center text-center fill-height">
      <h1 class="text-h3 font-weight-bold">امتیاز شما</h1>

      <div class="py-7" />

      <div class="text-h4 font-weight-light d-flex justify-center"><div class="mx-10">{{correctAnswers}} / {{ count }}</div><div >|</div><div class="mx-10">{{ Math.ceil(correctAnswers * 100 / count) }}%</div></div>
      <div class="py-7" />
      <div class="text-h5 font-weight-light">تعداد شرکت کنندگان: 10</div>

      <div class="py-15" />
      <v-btn class="w-100 mcolor" @click="resetQuiz()" :class="{'disable-events': name == ''}">
        بازگشت به صفحه اصلی
      </v-btn>
    </v-responsive>
  </v-container>
</template>

<style scoped>
.mcolor {
  background: #52b788;
  color: #0a1128;
}
.field {
  background: #00171f;
  color: aliceblue;
}
.hidden {
  display: none;
}
</style>

<style>
.green {
  color: green !important;
  background: rgba(0, 150, 0, 0.2) !important;
}
.red {
  color: red !important;
  background: rgba(150, 0, 0, 0.2) !important;
}
.disable-events {
  pointer-events: none
}
</style>