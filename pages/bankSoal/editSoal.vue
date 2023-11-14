<template>
  <main class="site-editSoal">
    <!-- <img src="~assets/img/bg.png" alt="" class="bgimage" /> -->
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>EDIT SOAL</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container flex-col-card">
        <div class="courcard-head cards-style">
          <h3 class="margin-text">Edit Soal</h3>
          <hr />
          <div class="form-soal">
            <form method="POST" @submit.prevent="onSubmit()">
              <div class="forms">
                <label for="namaMateri">Materi</label>
                <select id="namaMateri" v-model="dataSoal.subject" name="namaMateri">
                  <option :value="dataSoal.subject" selected disabled hidden>
                    {{ dataSoal.subject }}
                  </option>
                  <option value="Pengenalan Ilmu Kimia">
                    Pengenalan Ilmu Kimia
                  </option>
                </select>
              </div>
              <div class="forms">
                <label for="question">Soal</label>
                <textarea
                  id="question"
                  v-model="dataSoal.soal"
                  name="question"
                  rows="12"
                  cols="70"
                ></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansA">Jawaban A</label>
                <textarea id="ansA" v-model="dataSoal.ansA" name="ansA" rows="7" cols="70"></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansB">Jawaban B</label>
                <textarea id="ansB" v-model="dataSoal.ansB" name="ansB" rows="7" cols="70"></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansC">Jawaban C</label>
                <textarea id="ansC" v-model="dataSoal.ansC" name="ansC" rows="7" cols="70"></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="ansD">Jawaban D</label>
                <textarea id="ansD" v-model="dataSoal.ansD" name="ansD" rows="7" cols="70"></textarea
                ><br />
              </div>
              <div class="forms">
                <label for="correctAns">Jawaban Benar</label>
                <select id="correctAns" v-model="dataSoal.corAns" name="correctAns">
                  <option :value="dataSoal.corAns" selected disabled hidden>
                    {{ dataSoal.corAns }}
                  </option>
                  <option value="A">Jawaban A</option>
                  <option value="B">Jawaban B</option>
                  <option value="C">Jawaban C</option>
                  <option value="D">Jawaban D</option>
                </select>
              </div>
              <div class="forms">
                <label for="Level">Level</label>
                <select id="Level" v-model="dataSoal.level" name="Level">
                  <option :value="dataSoal.level" selected disabled hidden>
                    {{ dataSoal.level }}
                  </option>
                  <option value="1">Level 1</option>
                  <option value="2">Level 2</option>
                  <option value="3">Level 3</option>
                  <option value="4">Level 4</option>
                  <option value="5">Level 5</option>
                </select>
              </div>
              <button class="btn">Submit</button>
            </form>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
import Swal from 'sweetalert2'
import PageFooter from '~/components/PageFooter.vue'
import PagesBackground from '~/components/PagesBackground.vue';
export default {
  name: 'EditSoalPage',
  components: { PageFooter, PagesBackground },
  data() {
    return {
      dataSoal: [],
      API_URL: process.env.API_URL,
    }
  },

  async fetch() {
    await this.$axios(`api/banksoals/${this.$route.query.idSoal}`).then(
      (res) => {
        this.dataSoal = res.data.data
      }
    )
  },

  methods: {
    async onSubmit() {
      try {
        const form = {
          subject: this.dataSoal.subject,
          soal: this.dataSoal.soal,
          ansA: this.dataSoal.ansA,
          ansB: this.dataSoal.ansB,
          ansC: this.dataSoal.ansC,
          ansD: this.dataSoal.ansD,
          corAns: this.dataSoal.corAns,
          level: this.dataSoal.level,
        }
        await this.$axios.put(`${this.API_URL}/api/banksoals/${this.$route.query.idSoal}`,form
        )
        .then(res => {
          Swal.fire('Terima Kasih!', 'Soal Sudah Di Update', 'success')
          this.$router.push({name: 'TrialCode'})
        })
      } catch (error) {
        console.log(error)
      }
    },
  },

}
</script>

<style lang="scss" scoped>
.site-editSoal {
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
        margin-bottom: 50px;
        // margin-top: 50px;

        .form-soal {
          display: flex;
          justify-content: left;
          align-items: flex-start;
          margin-left: 50px;
          margin-bottom: 20px;

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
