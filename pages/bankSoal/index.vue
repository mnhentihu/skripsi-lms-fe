<template>
  <main class="bankSoal">
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
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div class="button-input">
            <button class="button-link">
              <nuxt-link class="link-input" to="bankSoal/inputSoal"
                ><Icon class="icon-link" icon="ant-design:plus-outlined" />
                Tambah Soal</nuxt-link
              >
            </button>
          </div>
          <div class="table">
            <table class="table-content">
              <thead>
                <tr>
                  <th style="width: 1%">Nomor</th>
                  <th style="width: 15%">Materi</th>
                  <th style="width: 30%">Pertanyaan</th>
                  <th style="width: 15%">Jawaban A</th>
                  <th style="width: 15%">Jawaban B</th>
                  <th style="width: 15%">Jawaban C</th>
                  <th style="width: 15%">Jawaban D</th>
                  <th style="width: 5%">Jawaban Benar</th>
                  <th style="width: 5%">Level</th>
                  <th style="width: 5%">Edit Soal</th>
                </tr>
              </thead>
              <tbody
                v-for="dataBanksoal in dataBanksoal"
                :key="dataBanksoal.id"
              >
                <tr>
                  <td>{{ dataBanksoal.id }}</td>
                  <td>{{ dataBanksoal.subject }}</td>
                  <td>{{ dataBanksoal.soal }}</td>
                  <td>{{ dataBanksoal.ansA }}</td>
                  <td>{{ dataBanksoal.ansB }}</td>
                  <td>{{ dataBanksoal.ansC }}</td>
                  <td>{{ dataBanksoal.ansD }}</td>
                  <td>{{ dataBanksoal.corAns }}</td>
                  <td>{{ dataBanksoal.level }}</td>
                  <td>
                    <button>
                      <nuxt-link
                        class="link-edit"
                        :to="{
                          name: 'TestingCode',
                          query: { idSoal: dataBanksoal.id },
                        }"
                        ><Icon
                          class="icon-dropdown"
                          icon="ant-design:edit-outlined"
                      /></nuxt-link>
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <PageFooter />
    </div>
  </main>
</template>

<script>
// import axios from '@nuxtjs/axios'
import { Icon } from '@iconify/vue2'
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'TestSoalPage',
  components: {
    Icon,
    PagesBackground,
  },

  data() {
    return {
      dataBanksoal: [],
      dropdown: false,
      API_URL: process.env.API_URL,
    }
  },

  async fetch() {
    await this.$axios(`api/banksoals`).then((res) => {
      this.dataBanksoal = res.data.data
    })
  },
}
</script>

<style lang="scss" scoped>
.bankSoal {
  display: flex;
  .left {
    width: auto;
  }
  .right {
    display: flex;
    flex-direction: column;
    max-width: 95.4%;

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
      width: 100%;
      min-height: 90vh;
      display: flex;
      justify-content: center;
      flex: 1;

      .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        // margin-top: 3rem;
        margin-bottom: 2rem;
        padding-bottom: 20px;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;
        .button-input {
          margin-top: 20px;
          width: 95%;

          .button-link {
            background-color: $secondary;
            width: 200px;
            height: 30px;
            border-radius: 20px;
            border: none;

            .link-input {
              display: flex;
              flex-direction: row;
              color: white;
              text-decoration: none;

              .icon-link {
                margin-right: 10px;
                margin-left: 10px;
                font-size: 12pt;
              }
            }
          }
        }
        .table {
          height: auto;
          width: 95%;
          border-radius: 20px;
          background-color: $secondary;
          margin-top: 20px;
          color: white;
          overflow-x: scroll;
          //   padding: 20px 20px 20px 20px;
          .table-content {
            border: 1px solid black;
            margin: 20px 20px 20px 20px;
            width: 2000px;
            // overflow-x: hidden;
            th,
            td {
              border: 1px solid black;
              padding: 10px 10px 10px 10px;
            }

            td {
              button {
                border: none;
                background: none;
                font-size: 16pt;
              }
            }
          }
        }
      }
    }
  }
}
</style>
