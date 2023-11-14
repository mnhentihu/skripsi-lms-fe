<template>
  <main class="site-materi">
    <PagesBackground />
    <div class="left">
      <SideBar />
    </div>
    <div class="right">
      <div class="header">
        <div class="head-text">
          <h1>Materi</h1>
        </div>
        <Profile />
      </div>
      <BreadCrumbs />
      <div class="container">
        <div class="content">
          <div
            v-if="data.role == 'Admin' || data.role == 'Guru'"
            class="button-input"
          >
            <button class="button-link">
              <nuxt-link class="link-input" to="materi/inputMateri"
                ><Icon class="icon-link" icon="ant-design:plus-outlined" />
                Tambah Materi</nuxt-link
              >
            </button>
          </div>
          <div
            v-for="dataMateri in dataMateri"
            :key="dataMateri.id"
            class="dropdown"
            :class="{
              'is-expanded': selectedDropdown === dataMateri.namaMateri,
            }"
          >
            <div class="title">
              <span>{{ dataMateri.namaMateri }}</span>
              <button @click.prevent="toggleDropdown(dataMateri.namaMateri)">
                <Icon class="icon-dropdown" icon="akar-icons:chevron-down" />
              </button>
            </div>
            <div class="desc">
              <span>{{ dataMateri.keterangan }}</span>
              <div class="wrapper">
                <Icon class="icon-materi" icon="ic:baseline-subject" />
                <nuxt-link
                  class="link-video"
                  :to="{
                    path: '/materi/videoMateri',
                    query: { id_Video: dataMateri.id },
                  }"
                  >Video Materi</nuxt-link
                >
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
import PagesBackground from '~/components/PagesBackground.vue'
export default {
  name: 'MateriPage',
  components: {
    Icon,
    PagesBackground,
  },
  data() {
    return {
      selectedDropdown: null,
      dummy: 2,
      dataMateri: [],
      data: [],
    }
  },

  async fetch() {
    await this.$axios.get(`api/materis`).then((res) => {
      this.dataMateri = res.data.data
    })
  },

  mounted() {
    if (localStorage.getItem('loginData')) {
      try {
        this.data = JSON.parse(localStorage.getItem('loginData'))
      } catch (e) {
        localStorage.removeItem('loginData')
      }
    }
  },

  methods: {
    toggleDropdown(namaMateri) {
      try {
        if (
          this.selectedDropdown == null ||
          this.selectedDropdown !== namaMateri
        ) {
          this.selectedDropdown = namaMateri
          console.log('sukses', namaMateri)
          console.log(this.selectedDropdown)
        } else {
          this.selectedDropdown = null
          console.log('gagal', namaMateri)
          console.log(this.selectedDropdown)
        }
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.site-materi {
  display: flex;
  .left {
    width: auto;
  }
  .right {
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
              flex-direction: row;
              align-items: center;
              .icon-materi {
                font-size: 40px;
              }
              .link-video {
                text-decoration: none;
                color: white;
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
}
</style>
