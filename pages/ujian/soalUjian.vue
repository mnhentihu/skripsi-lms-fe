<template>
  <main class="site-materi">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>Bank Soal</h1>
        </div>
        <Profile />
      </div>
      <div class="container">
        <div class="content" :class="{ 'is-expanded': dropdown }">
          <div class="soal-ujian">
            <div class="number">
              <span>{{ activeState + 1 }}</span>
            </div>
            <div class="question">
              <span>{{ dataSoal[activeState]?.soal }}</span>
              <!-- <p>{{ answer[activeState] }}</p> -->
              <div class="selection">
                <div class="select">
                  <p>A.</p>
                  <input
                    id="ansA"
                    v-model="answer[activeState]"
                    type="radio"
                    value="A"
                    @change="isiResult"
                  />
                  <label for="ansA">
                    {{ dataSoal[activeState]?.ansA }}
                  </label>
                </div>
                <div class="select">
                  <p>B.</p>
                  <input
                    id="ansB"
                    v-model="answer[activeState]"
                    type="radio"
                    value="B"
                    @change="isiResult"
                  />
                  <label for="ansB">
                    {{ dataSoal[activeState]?.ansB }}
                  </label>
                </div>
                <div class="select">
                  <p>C.</p>
                  <input
                    id="ansC"
                    v-model="answer[activeState]"
                    type="radio"
                    value="C"
                    @change="isiResult"
                  />
                  <label for="ansC">
                    {{ dataSoal[activeState]?.ansC }}
                  </label>
                </div>
                <div class="select">
                  <p>D.</p>
                  <input
                    id="ansD"
                    v-model="answer[activeState]"
                    type="radio"
                    value="D"
                    @change="isiResult"
                  />
                  <label for="ansD">
                    {{ dataSoal[activeState]?.ansD }}
                  </label>
                </div>
              </div>
            </div>
          </div>
          <div class="navigation">
            <div class="navigation-number">
              <select
                id="navi"
                v-model="navigasi"
                class="navi"
                name="navi"
                @change="navSoal(navigasi)"
              >
                <option selected="true" disabled="disabled" value="">
                  Nomor Soal
                </option>
                <option
                  v-for="(data, index) in dataSoal"
                  :key="index"
                  :value="index"
                >
                  Nomor {{ index + 1 }}
                </option>
              </select>
            </div>
            <div class="navigation-button">
              <button
                class="prev"
                :disabled="activeState == 0 ? true : false"
                @click="prevButton"
              >
                <Icon
                  class="icon-prev"
                  icon="ooui:previous-ltr"
                  style="color: white"
                />
              </button>
              <button
                class="next"
                :disabled="activeState == dataSoal.length - 1 ? true : false"
                @click="nextButton"
              >
                <Icon
                  class="icon-next"
                  icon="ooui:previous-rtl"
                  style="color: white"
                />
              </button>
            </div>
            <div class="submit-button">
              <button class="submit" @click="onSubmit">Submit</button>
            </div>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import { Icon } from '@iconify/vue2'
// import SideBar from '../../components/SideBar.vue'
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'TestSoalPage',
  components: {
    Icon,
    PagesBackground,
  },
  data() {
    return {
      navigasi: '',
      activeState: 0,
      dropdown: false,
      dataSoal: [],
      answer: [],
      isiJawaban: [],
      // finalResult: [],
      idSiswa: JSON.parse(localStorage.getItem('loginData')).id,
      API_URL: process.env.API_URL,
    }
  },

  async mounted() {
    await this.$axios
      .get(
        `api/exam/${this.$route.query[0].id_user}/${this.$route.query[1].level}/${this.$route.query[2].subject}`
      )
      .then((res) => {
        this.dataSoal = res.data.data[0].id_soal
        this.dataExam = res.data.data[0]
        this.isiJawaban.push(this.dataExam.jawaban)
        // if ( this.isiJawaban == null){
        //   this.answer = []
        // } else {
        this.answer = this.isiJawaban[0]
        // }
        console.log(this.answer)
      })
      .catch((error) => {
        console.log(error)
      })
  },

  methods: {
    nextButton() {
      this.activeState++
      // this.answer = this.isiJawaban[this.activeState]
      console.log(this.answer[this.activeState])
    },
    prevButton() {
      this.activeState--
      // this.answer = this.isiJawaban[this.activeState]
      console.log(this.answer[this.activeState])
    },
    navSoal(navigasi) {
      this.activeState = navigasi
    },
    isiResult() {
      this.isiJawaban[this.activeState] = this.answer[this.activeState]
      console.log("isi jawaban : ", this.isiJawaban)
    },
    async onSubmit() {
      try {
        await this.$axios
          .put(`${this.API_URL}/api/exam`, {
            id: this.dataExam.id,
            id_user: this.idSiswa,
            level: this.dataExam.level,
            subject: this.dataExam.subject,
            jawaban: this.isiJawaban,
          })
          .then((res) => {
            console.log(res)
            console.log(JSON.stringify(this.isiJawaban))
            this.$router.push({ path: '/ujian' })
          })
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.site-materi {
  display: flex;
  height: 100vh;

  .left {
    width: auto;
  }
  .right {
    height: auto;
    display: flex;
    flex-direction: column;

    .header {
      display: flex;
      justify-content: space-between;
      border-bottom: solid 2px $secondary;
      box-shadow: 2px 2px 30px 2px #888;

      .head-text {
        margin-left: 20px;
        color: $secondary;
      }
    }
    .container {
      height: auto;
      display: flex;
      justify-content: center;
      flex: 1;

      .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 3rem;
        margin-bottom: 2rem;
        padding-bottom: 20px;
        min-height: 250px !important;
        // height: 250px;
        max-height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .soal-ujian {
          display: flex;
          flex-direction: row;
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: $secondary;
          margin-top: 20px;
          color: white;

          .number {
            display: flex;
            align-items: center;
            width: 50px;
            border-right: 1px solid black;

            span {
              margin: auto;
            }
          }

          .question {
            padding: 10px 20px 10px 20px;

            span {
              // background-color: pink;
              text-align: justify;
              // text-justify: inter-word;
            }

            .selection {
              display: flex;
              flex-direction: column;
              align-items: center;
              margin-top: 20px;

              .select {
                display: flex;
                align-items: center;
              }

              .ansA,
              .ansB,
              .ansC,
              .ansD {
                display: flex;
              }
              .icon-materi {
                font-size: 40px;
              }
              .link-video {
                text-decoration: none;
                color: white;
              }
            }
          }
        }
        .navigation {
          display: flex;
          justify-content: space-between;
          width: 90%;
          margin-top: 40px;

          .navigation-number {
            width: 400px;
            select {
              color: white;
              background-color: $secondary;
              border-radius: 20px;
              width: 150px;
              height: 30px;
              font-size: 12pt;
              padding-left: 10px;
            }
          }

          .navigation-button {
            color: white;
            display: flex;
            justify-content: flex-start;

            button {
              font-size: 12pt;
              border-radius: 20px;
              background-color: $secondary;
              width: 100px;
              border: none;
            }

            button:disabled {
              background-color: #acafb0;
            }

            button:hover {
              border: solid 2px $primary;
              color: $primary;
            }

            .prev {
              margin-right: 20px;
            }
          }

          .submit-button {
            display: flex;
            justify-content: flex-end;
            .submit {
              color: white;
              font-size: 12pt;
              border-radius: 20px;
              background-color: $secondary;
              border: none;
              width: 100px;
              height: 30px;
            }

            .submit:hover {
              border: solid 2px $primary;
              color: $primary;
            }
          }
        }
      }
    }
  }
}
</style>
