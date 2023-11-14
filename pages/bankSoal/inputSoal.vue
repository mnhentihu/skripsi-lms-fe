<template>
  <main class="site-inputSoal">
    <!-- <img src="~assets/img/bg.png" alt="" class="bgimage" /> -->
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>INPUT SOAL</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="courcard-head cards-style">
          <div class="form-soal">
            <form method="POST" @submit.prevent="onSubmit()">
              <div class="forms">
                <label for="namaMateri">Materi</label>
                <select id="namaMateri" v-model="subject" name="namaMateri">
                  <option selected="true" disabled="disabled" value="" hidden>
                    Pilih Materi
                  </option>
                  <option v-for="dataMateri in dataMateri" :key="dataMateri.id" :value="dataMateri.namaMateri">
                    {{ dataMateri.namaMateri}}
                  </option>
                </select>
              </div>
              <div class="forms">
                <label for="question">Soal</label>
                <textarea
                  id="question"
                  v-model="soal"
                  name="question"
                  rows="12"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansA">Jawaban A</label>
                <textarea
                  id="ansA"
                  v-model="ansA"
                  name="ansA"
                  rows="7"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansB">Jawaban B</label>
                <textarea
                  id="ansB"
                  v-model="ansB"
                  name="ansB"
                  rows="7"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansC">Jawaban C</label>
                <textarea
                  id="ansC"
                  v-model="ansC"
                  name="ansC"
                  rows="7"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansD">Jawaban D</label>
                <textarea
                  id="ansD"
                  v-model="ansD"
                  name="ansD"
                  rows="7"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="correctAns">Jawaban Benar</label>
                <select id="correctAns" v-model="corAns" name="correctAns">
                  <option selected="true" disabled="disabled" value="" hidden>
                    Pilih Jawaban Benar
                  </option>
                  <option value="A">Jawaban A</option>
                  <option value="B">Jawaban B</option>
                  <option value="C">Jawaban C</option>
                  <option value="D">Jawaban D</option>
                </select>
              </div>
              <div class="forms">
                <label for="Level">Level</label>
                <select id="Level" v-model="level" name="Level">
                  <option selected="true" disabled="disabled" value="" hidden>
                    Pilih Level
                  </option>
                  <option value="easy">Easy</option>
                  <option value="medium">Medium</option>
                  <option value="Hard">Hard</option>
                </select>
              </div>
              <!-- <div v-if="isSuccess" class="success" >Soal Berhasil Ditambahkan</div> -->
              <button type="submit"  class="btn">Save</button>
            </form>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import PageFooter from '~/components/PageFooter.vue'
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'InputSoalPage',
  components: { PageFooter, PagesBackground },

  data() {
    return {
      // isSuccess: false,
      subject: '',
      soal: '',
      ansA: '',
      ansB: '',
      ansC: '',
      ansD: '',
      corAns: '',
      level: '',
      dataMateri: [],
      API_URL: process.env.API_URL,
    }
  },

  async fetch() {
    await this.$axios(`api/materis`).then((res) => {
      this.dataMateri = res.data.data
    })
  },

  methods: {
    async onSubmit() {
      try {
        const form = {
          subject: this.subject,
          soal: this.soal,
          ansA: this.ansA,
          ansB: this.ansB,
          ansC: this.ansC,
          ansD: this.ansD,
          corAns: this.corAns,
          level: this.level,
        }
        await axios.post(`${this.API_URL}/api/banksoals`,
          form
        )
        .then(res => {
          Swal.fire('Terima Kasih!', 'Materi Sudah Di Upload', 'success')
          this.$router.push({path: '/bankSoal'})
        })
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.site-inputSoal {
  display: flex;
  position: relative;
  .left {
    width: auto;
  }
  .right {
    display: flex;
    flex-direction: column;
    .header {
      display: flex;
      justify-content: space-between;
      border-bottom: solid 1px $secondary;
      box-shadow: 2px 2px 30px 2px #888;

      .head-text {
        margin-left: 20px;
        color: $secondary;
      }
    }
    .container {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 100%;

      .courcard-head {
        width: 95%;
        // height: 250px;
        // margin-top: 50px;
        margin-bottom: 50px;
        background-color: $secondary;
        color: white;
        font-size: 12pt;

        .form-soal {
          display: flex;
          justify-content: left;
          align-items: flex-start;
          margin-left: 50px;
          margin-bottom: 20px;
          margin-top: 30px;

          .forms {
            display: flex;
            flex-direction: row;
            align-items: flex-start;

            label {
              width: 150px;
              clear: left;
              text-align: left;
              padding-right: 10px;
              font-weight: 500;
            }

            textarea,
            input,
            select {
              margin-bottom: 20px;
              border-radius: 20px;
            }

            select{
              height: auto;
              width: auto;
              font-size: 12pt;
              padding: 10px;
            }
          }
        }
      }
    }
  }

  hr {
    border-width: 2px;
  }
}
</style>