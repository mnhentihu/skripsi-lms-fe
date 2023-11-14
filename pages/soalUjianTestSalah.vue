<template>
  <main class="site-ujian">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>UJIAN</h1>
        </div>
        <Profile />
      </div>
      <div class="container">
        <div class="content">
          <div
            v-for="(data, index) in dataMateri"
            :key="index"
            class="dropdown"
            :class="{ 'is-expanded': selectedDropdown === data.namaMateri }"
          >
            <div class="title">
              <span>{{ data.namaMateri }}</span>
              <button @click.prevent="toggleDropdown(data.namaMateri)">
                <Icon class="icon-dropdown" icon="akar-icons:chevron-down" />
              </button>
            </div>
            <div v-if="selectedDropdown === data.namaMateri" class="desc">
              <div class="wrapper">
                <button
                  class="link-ujian"
                  @click="ujianMulai(data.namaMateri, '1')"
                >
                  Level 1
                </button>
                <button
                  class="link-ujian"
                  @click="ujianMulai(data.namaMateri, '2')"
                >
                  Level 2
                </button>
                <button
                  class="link-ujian"
                  @click="ujianMulai(data.namaMateri, '3')"
                >
                  Level 3
                </button>
                <button
                  class="link-ujian"
                  @click="ujianMulai(data.namaMateri, '4')"
                >
                  Level 4
                </button>
                <button
                  class="link-ujian"
                  @click="ujianMulai(data.namaMateri, '5')"
                >
                  Level 5
                </button>
                <button
                  class="link-ujian"
                  style="margin-bottom: 10px"
                  @click="ujianMulai(data.namaMateri, '1')"
                >
                  Final Test
                </button>
              </div>
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
import axios from 'axios'
export default {
  name: 'AssignmentPage',
  components: {
    Icon,
  },
  data() {
    return {
      selectedDropdown: null,
      dataMateri: [],
      userId: '',
    }
  },

  async fetch() {
    await this.$axios.get(`api/materis`).then((res) => {
      this.dataMateri = res.data.data
      // console.log(this.dataMateri)
    })
  },

  mounted() {
    this.userId = JSON.parse(localStorage.getItem('loginData')).id
  },

  methods: {
    toggleDropdown(namaMateri) {
      try {
        if (
          this.selectedDropdown == null ||
          this.selectedDropdown !== namaMateri
        ) {
          this.selectedDropdown = namaMateri
          console.log( 'sukses' ,namaMateri)
        } else {
          this.selectedDropdown = null
          console.log('gagal',namaMateri)
        }
      } catch (error) {
        console.error(error)
      }
    },

    async ujianMulai(materi, syulit) {
      try {
        const form = {
          id_user: this.userId,
          level: syulit,
          subject: materi,
        }
        await axios
          .post(`${this.API_URL}/api/exam/`, form)
          .then((res) => {
            localStorage.setItem('dataSoal', JSON.stringify(res.data))
            this.$router.push({
              path: '/ujian/soalUjian',
              query: [
                { id_user: res.data[0].id_user },
                { level: res.data[0].level },
                { subject: res.data[0].subject },
              ],
            })
            // axios.get(`api/exam/$re{id_user}/{level}/{subject}`)
          })
      } catch (error) {
        console.log(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.site-ujian {
  display: flex;
  .left {
    width: auto;
  }
  .right {
    position: relative;
    display: flex;
    flex-direction: column;
    // flex: 1;

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
      align-items: flex-start;
      flex: 1;

      .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 3rem;
        margin-bottom: 2rem;
        padding-bottom: 20px;
        height: auto;
        width: 95%;
        border: none;
        border-radius: 20px;
        box-shadow: 10px 10px 30px 15px #888;

        .dropdown {
          height: auto;
          width: 90%;
          border-radius: 20px;
          background-color: $secondary;
          margin-top: 20px;
          color: white;

          .title {
            padding: 10px 0px 10px 0px;
            display: flex;
            justify-content: space-between;

            button {
              background: none;
              border: none;
              margin-right: 10px;
              cursor: pointer;

              .icon-dropdown {
                font-size: 15pt;
                font-weight: bold;
                color: white;
              }
            }

            span {
              padding: 0px 20px 0px 30px;
            }
          }

          .desc {
            display: none;
            border-top: solid 1px white;
            padding: 10px 0px 10px 0px;

            span,
            .wrapper {
              padding: 0px 20px 0px 30px;
            }

            .wrapper {
              display: flex;
              flex-direction: column;
              align-items: flex-start;
              .icon-materi {
                font-size: 40px;
              }
              .link-ujian {
                background-color: transparent;
                border: none;
                color: white;
                margin-top: 15px;
                cursor: pointer;
              }
              .final {
                margin-bottom: 10px;
              }
            }
          }
        }

        &.is-expanded {
          .desc {
            display: flex;
            flex-direction: column;
          }
        }
      }
    }
  }
}
</style>
